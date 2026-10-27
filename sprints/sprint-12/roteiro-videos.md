# Roteiro dos vídeos do BuscaPet (Sprint 12)

Dois vídeos obrigatórios, conforme o roteiro da disciplina:

| Vídeo | Formato | Duração | Onde publicar |
|---|---|---|---|
| Apresentação completa | horizontal 16:9 | até 10 min | YouTube |
| Divulgação | vertical 9:16 | 30 a 45 s | Instagram, marcando **@pryscillabgoncalves** e **@antenorparnaiba** |

## Preparação (vale para os dois)

1. Abrir o Docker Desktop e rodar `.\iniciar-demo.ps1` na pasta do `BuscaPet-App` (ver o README, "Início rápido").
2. Conferir: interface em `http://localhost:5173`, `http://localhost:8000/health/db` com `"conectado"` e, no
   Gerenciador de Tarefas, a aba Desempenho → CPU → **Processadores lógicos** aberta numa janela ao lado (mostra os
   núcleos trabalhando durante o benchmark).
3. Contas (senha de todas: `Senha1234!`): `maria@exemplo.com` (tutora, tem um cão perdido), `joao@exemplo.com`
   (tutor, tem um pastor perdido), `patinhas@exemplo.com` (abrigo verificado), `amigofiel@exemplo.com` (apoiador),
   `admin@exemplo.com` (administração).
4. Gravar em 1920×1080, zoom do navegador em 100% a 110%, sem outras abas abertas. Para o vídeo vertical, usar o
   modo celular do navegador (F12 → Ctrl+Shift+M) ou o PWA instalado no celular.
5. Plano B: se algo falhar na gravação, `docker compose restart` e rodar `.\iniciar-demo.ps1` de novo (não duplica dados).

## Vídeo 1 — Apresentação completa (16:9, até 10 min)

Estrutura sugerida (tempos somam 9 min 30 s, deixando folga):

| Tempo | Bloco | O que mostrar na tela | O que dizer |
|---|---|---|---|
| 0:00 – 1:00 | **O problema** | Slide simples ou página inicial | Todo ano muitos animais se perdem e muitos esperam adoção em abrigos. As informações estão espalhadas em redes sociais, sem mapa e sem forma de comparar fotos. Quem perdeu o bichinho não consegue saber se ele está num abrigo ou foi visto na rua. |
| 1:00 – 2:00 | **A solução** | Página inicial, menu, mapa | BuscaPet é uma plataforma web com três módulos: adoção, reencontro de animais perdidos por semelhança visual e doações aos abrigos. Funciona no computador e no celular (PWA instalável). |
| 2:00 – 3:00 | **Arquitetura e tecnologias** | Diagrama de integração (`BuscaPet-Docs`, manual técnico) | Interface em React e Vite; API em Python com FastAPI; banco PostgreSQL; **núcleo de comparação de imagens em C com OpenCL**, rodando como serviço separado; Docker para empacotar; testes automatizados e CI no GitHub. |
| 3:00 – 4:00 | **Adoção** | Visitante: Mapa → Animais → ficha do Thor. Entrar como `joao@` → **Tenho interesse**. Entrar como `patinhas@` → sino → **Interessados** → aprovar. | O visitante navega sem conta; para agir precisa entrar. O abrigo verificado recebe o aviso e decide. |
| 4:00 – 6:00 | **Perdidos e núcleo OpenCL (destaque)** | Entrar como `maria@` → **Meus registros** → cão perdido → **Registros parecidos**. Mostrar o avistamento do João em 1º com semelhança **alta**, e os abrigos abaixo. Entrar como `joao@` → o pastor perdido → o **Rex**, do Protetores do Cordeiro, em 1º. | O tutor envia fotos; o sistema extrai um descritor de cor, textura e proporções de cada foto e compara com todas as outras. Nunca afirma que é o mesmo animal: mostra candidatos por grau de semelhança, e a decisão é do tutor. |
| 6:00 – 7:00 | **Doações** | `patinhas@` → Necessidades e doações → **Meta do mês**. `amigofiel@` → mural → doar. Voltar ao Patinhas e **confirmar**. Mostrar o ranking em **Quem apoia**. | A doação só conta depois que o abrigo confirma. O PIX é do próprio abrigo, nada é pago dentro da plataforma. A meta do mês sobe a cada confirmação. |
| 7:00 – 8:30 | **Núcleo OpenCL: sequencial × paralelo** | `admin@` → aba **Núcleo OpenCL** → **Rodar a curva**. Ao lado, o Gerenciador de Tarefas com os processadores lógicos subindo. | A mesma comparação feita de dois jeitos, com resultados idênticos bit a bit. Com OpenCL nos núcleos da CPU, a busca em 100 mil fotos fica cerca de 3,4 vezes mais rápida. (Usar os números que a tela mostrar na gravação.) |
| 8:30 – 9:30 | **Qualidade e encerramento** | Terminal com `pytest` (249 testes, 94%) ou a página de Actions verde; repositório no GitHub | 249 testes na API, 91 cenários ponta a ponta, acessibilidade WCAG 2.1 AA, documentação da API gerada do código. Agradecimento e a equipe. |

Dicas de gravação: um bloco por vez (facilita refazer), falar sem pressa, e tirar do ar notificações do sistema.
Deixar o cursor visível e dar zoom nos pontos importantes na edição (a nota "alta" e o gráfico do benchmark).

## Vídeo 2 — Divulgação (9:16, 30 a 45 s)

Ideia: começar pelo gancho emocional, mostrar a "mágica" e terminar com a chamada.

| Tempo | Cena | Texto na tela / fala |
|---|---|---|
| 0:00 – 0:04 | Foto de um cão olhando para a câmera, com a legenda | "Perdeu seu pet?" |
| 0:04 – 0:12 | Celular: tela **Perdi meu animal**, escolhendo a foto | "Envie uma foto." |
| 0:12 – 0:22 | Tela de resultados: o cão certo em 1º, selo **Semelhança alta** | "O BuscaPet compara com animais de abrigos e avistamentos da cidade." |
| 0:22 – 0:30 | Corte rápido: gráfico do benchmark e Gerenciador de Tarefas com os núcleos acesos | "Processamento paralelo em OpenCL." |
| 0:30 – 0:38 | Mapa com os abrigos e o mural de doações | "Adote. Reencontre. Ajude." |
| 0:38 – 0:45 | Logo e nome do projeto | "BuscaPet — UNINASSAU" |

Legenda sugerida para a publicação:

> Perdeu seu pet? Tire uma foto e o BuscaPet procura nos abrigos e avistamentos da cidade, usando comparação de
> imagens com processamento paralelo (OpenCL). Projeto de Fábrica de Software e Tópicos Avançados, UNINASSAU.
> @pryscillabgoncalves @antenorparnaiba #BuscaPet #AdoteUmPet #OpenCL #UNINASSAU

Checklist antes de publicar: marcar os dois professores **na legenda e no próprio vídeo (marcar pessoas)**, conferir
que o vídeo é vertical 1080×1920, tem no máximo 90 s e está público. Guardar o link para colocar no documento da
Sprint 12 e na entrega do Teams (o vídeo horizontal vai no YouTube, também com o link na entrega).

## Andamento (preencher na gravação)

| Etapa | Vídeo 16:9 | Vídeo 9:16 |
|---|---|---|
| Roteiro | pronto (este arquivo) | pronto (este arquivo) |
| Gravação | | |
| Edição | | |
| Publicação e link | | |
