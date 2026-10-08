# Sprint 3 (oficial) — resumo em texto

Versão em markdown do documento entregue (`BuscaPet-Sprint3.pdf` / `.docx`, nesta pasta, com as
figuras). Serve para leitura rápida e para o Claude de quem chega ao projeto. Se houver divergência,
vale o PDF entregue. Última revisão: 2026-10-08.

**Entregue e enviada à professora.** Código no `main` de
[`BuscaPet-App`](https://github.com/Equipe-BuscaPet/BuscaPet-App).

## O que a Sprint pedia → o que foi feito

| Exigência | Resultado |
|---|---|
| Banco de dados conectado | PostgreSQL no Supabase, 19 tabelas de domínio (migration `0001`). Prova: `GET /health/db` executa `SELECT 1` de verdade. |
| Login funcional | `POST /auth/login`, login único para todos os perfis, JWT de 24 h. |
| Cadastro persistido | Tutor, abrigo e apoiador, conta + perfil na mesma transação. Admin só por script. |
| Perfis e permissões | Quatro perfis; a regra está na API (`exigir_tipo`, `get_abrigo_aprovado`), a interface só reflete. |
| CRUD da entidade principal | **Animal** para adoção: criar, listar com filtros, ver ficha, editar parcial, excluir. |
| Primeiro deploy local | API + interface locais, dados no Supabase. Passo a passo no `README.md` do `BuscaPet-App`. |

## Decisões de segurança e de projeto (e por quê)

- **Senha com bcrypt**, entre 8 e 72 bytes (o bcrypt só olha os 72 primeiros; recusar acima evita
  duas senhas diferentes com o mesmo hash).
- **Mensagem única de falha de login** ("E-mail ou senha incorretos") para e-mail inexistente, senha
  errada ou conta desativada, e comparação com hash falso para não vazar pelo tempo de resposta.
- **O token não basta:** o usuário é relido do banco a cada requisição, então desativar a conta
  invalida o token na hora.
- **E-mail** em minúsculas e único (409 se repetir). **CNPJ** validado pelos dígitos verificadores.
- **Exclusão de conta é lógica** (desativa): doações, interesses e logs apontam para o usuário.
- **401** sem login válido, **403** com login mas sem permissão, **404** para animal que a pessoa não
  pode ver (não confirma a existência de cadastro não validado).
- **Exclusão de animal é real**, mas remove antes interesses, fotos e descritores; se houver
  correspondências do reencontro, responde 409 e orienta a marcar como "adotado" ou "transferido".
- Situações do animal: disponível, em processo de adoção, adotado, óbito, transferido. O catálogo mostra
  só os disponíveis, de abrigos aprovados.

## Quem pode fazer o quê (Sprint 3)

| Ação | Visitante | Tutor | Abrigo pendente | Abrigo aprovado | Apoiador | Admin |
|---|---|---|---|---|---|---|
| Ver catálogo e ficha | sim | sim | sim | sim | sim | sim |
| Cadastrar animal | — | — | — | sim | — | — |
| Editar animal | — | — | — | só os seus | — | — |
| Excluir animal | — | — | — | só os seus | — | sim |
| Aprovar/rejeitar abrigos | — | — | — | — | — | sim |
| Consultar/alterar a própria conta | — | sim | sim | sim | sim | sim |

> **Isto vai mudar na Sprint 4:** o abrigo deixa de depender de aprovação para operar (ver
> `CONTEXTO.md`, seção 4). A coluna "Abrigo pendente" passa a significar "não verificado" e o admin
> só concede o selo.

## Lição aprendida (importante para quem mexe no banco)

Os testes rodam em SQLite e **não pegaram** um bug de enums: o SQLAlchemy gravava o *nome* do valor
(`ADMIN`) e o Postgres aceita o *valor* (`admin`). Todo cadastro falhava no Supabase, mas passava nos
testes. Corrigido num ponto único (`pg_enum` em `api/app/models/enums.py`, 20 colunas). **Regra da
equipe: a validação final de qualquer mudança de banco roda num Postgres real** (o do Docker serve).

## Verificação feita

37 testes da API + `ruff` limpo; fluxo da API 17/17 no Supabase real; interface 22/22 (navegador,
inclusive em tela de celular); CI do GitHub (API e frontend) aprovado.

## Limitações declaradas no PDF e como ficaram depois

| Limitação no PDF | Situação em 2026-10-08 |
|---|---|
| Docker Compose nunca executado | **Resolvido.** Validado (`db`, `redis`, `api`, `frontend`): migrations aplicadas, cadastro e login funcionando. |
| `alembic check` com divergências | **Resolvido** pela migration `0002_alinha_models_e_banco`. |
| Fotos e telas de perfil de tutor/apoiador | Continuam fora; fotos entram com o núcleo de similaridade. |
| Núcleo OpenCL sem implementação | Continua. Fora das entregas oficiais de Fábrica de Software; obrigatório em Tópicos Avançados. |

## Para a Sprint 4

Ver o plano em `CONTEXTO.md` do `BuscaPet-App` (módulo Adoção: interesse do tutor, resposta do abrigo,
validações, mensagens de erro amigáveis, selo "Verificado" no lugar da aprovação obrigatória).
