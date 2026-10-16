# Núcleo OpenCL: spike e esqueleto do serviço

**Plano interno:** Sprint 1 (gate do spike) + Sprint 3, semana 5 (estrutura do serviço `/nucleo-opencl`).
**Data:** 16/10/2026 · **Responsável:** Kaian (P2), apoio ao núcleo · **Dono do núcleo:** Nivaldo (P1)
**Código:** `BuscaPet-App/nucleo-opencl/` · **Requisitos atendidos:** base para RF-21 a RF-29

---

## 1. Por que este passo veio primeiro

O plano interno pedia, ainda na Sprint 1, um **spike**: um kernel trivial (soma de vetores) rodando via
PoCL, com o resultado batendo com a versão em CPU pura e o tempo do kernel medido separado do tempo de
transferência. O próprio plano diz que *"nada mais avança sem essa confirmação"*. Até 16/10 a pasta
`nucleo-opencl/` tinha só um Dockerfile que rodava `clinfo`, e os kernels da semana 6 (pré-processamento e
descritores) não teriam onde rodar nem como ser testados. Por isso o spike e o esqueleto do serviço
vieram antes de qualquer outra tarefa do núcleo.

## 2. O que foi entregue

| Entrega | Onde |
|---|---|
| Kernel `soma_vetores` | `kernels/soma_vetores.cl` |
| Programa do spike: compara CPU sequencial × OpenCL e mede envio, kernel e retorno | `src/spike.c` |
| Utilidades de OpenCL reaproveitáveis pelos próximos kernels | `src/ocl.c`, `src/ocl.h` |
| Serviço HTTP com `GET /saude`; rotas do contrato respondem `501` | `src/servidor.c` |
| `make`, `make teste`, `make limpar` | `Makefile` |
| Imagem em dois estágios; o build falha se o teste falhar | `Dockerfile` |
| Job `nucleo-opencl` na CI | `.github/workflows/ci.yml` |

## 3. Decisões e o porquê

| Decisão | Alternativas consideradas | Por que esta |
|---|---|---|
| **C puro + `CL/cl.h`** | PyOpenCL, C++ | Já fechada no CONTEXTO §5; a disciplina avalia o uso direto da API do OpenCL. |
| **PoCL na CPU** | Driver de GPU | Ninguém do time tem GPU compatível garantida; o PoCL roda igual em qualquer máquina, na CI e no deploy gratuito. |
| **`libmicrohttpd` para HTTP** | Servidor com *sockets* escrito à mão; Mongoose | É biblioteca madura do projeto GNU e já vem empacotada no Debian (`libmicrohttpd-dev`). Escrever HTTP à mão gastaria o tempo da janela crítica com algo que não é o foco da disciplina. |
| **`cJSON` para JSON** | Montar JSON com `printf` | Evita JSON quebrado por aspas ou acentos e também está empacotada no Debian. |
| **Kernels em arquivos `.cl` lidos na partida** | Kernel embutido como *string* no C | O `.cl` fica legível, ganha realce de sintaxe e o erro de compilação aponta a linha certa. A pasta é trocável por `NUCLEO_KERNELS`. |
| **Docker multi-stage com `make teste` no build** | Testar só na CI | Ninguém consegue gerar (nem publicar) uma imagem com kernel errado, nem na própria máquina. |
| **Usuário sem root na imagem final** | Rodar como root | Boa prática de segurança; o serviço não precisa de privilégio nenhum. |
| **Erros no formato `{"detail": ...}`** | Formato próprio | É o mesmo formato do FastAPI, então o backend trata erro do núcleo e erro próprio do mesmo jeito. |

## 4. Gate de saída da Sprint 1 (plano interno), item do OpenCL

- [x] Kernel OpenCL executando via PoCL: validado na máquina do Kaian (i5-2400) e na CI (Ubuntu do GitHub).
- [ ] Validar na máquina do Nivaldo e na do Marlon: `docker compose up -d --build nucleo-opencl` e
      `curl localhost:8001/saude`. Como tudo roda no container, a expectativa é funcionar igual.
- [x] Resultado bate com a CPU pura: `make teste` com N = 1, 1.000.003 (primo) e 4.194.304.
- [x] Tempo do kernel medido separado do tempo de transferência (eventos de *profiling* do OpenCL).

## 5. Primeira medição

i5-2400 (4 núcleos, sem GPU), N = 4.194.304 floats (16 MB por vetor), 16/10/2026:

| Etapa | Tempo (ms) |
|---|---:|
| CPU sequencial (referência) | 16,9 |
| OpenCL: envio (host → device) | 29,1 |
| OpenCL: kernel | 8,9 |
| OpenCL: retorno (device → host) | 4,6 |
| OpenCL: total | 43,5 |

**Interpretação.** O kernel sozinho foi ~1,9× mais rápido que o laço sequencial, porque usou os 4 núcleos.
Ainda assim, de ponta a ponta o OpenCL ficou ~2,6× **mais lento**: a soma de vetores faz uma única conta por
número copiado, então a transferência (34 ms) pesa mais que o cálculo. Isto **não** é um problema do
núcleo. É a demonstração prática da Lei de Amdahl que o benchmark do RF-29 vai explorar: o paralelismo
compensa quando há muito cálculo por dado transferido, como na extração de descritores (vizinhanças de
cada pixel) e na comparação de uma foto contra milhares de candidatos. Os números servem como **linha de
base** para comparar com os kernels reais.

## 6. Como reproduzir

```bash
cd BuscaPet-App
docker compose up -d --build nucleo-opencl      # compila, roda make teste, sobe o serviço
curl localhost:8001/saude                       # qual dispositivo OpenCL o serviço enxerga
docker compose run --rm nucleo-opencl ./spike 10000000   # spike com outro tamanho
```

## 7. Próximos passos (Sprint 3, semanas 6 e 7)

1. Extração de descritores **sequencial** em C (histograma de cor, textura, proporções), que é a
   referência para os testes. É tarefa do Nivaldo (dono) em pareamento.
2. Kernels de pré-processamento e de descritores, cada um com o seu teste de equivalência no `make teste`.
3. Kernel de comparação em massa e as rotas `POST /extrair-descritor` e `POST /comparar`.
4. Backend chamando o núcleo via `httpx` (RF-21/22/23).
