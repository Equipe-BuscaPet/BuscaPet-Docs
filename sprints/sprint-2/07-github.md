# 7. Projeto estruturado no GitHub

Organização: **https://github.com/Equipe-BuscaPet**, com 3 repositórios:

- [`BuscaPet-App`](https://github.com/Equipe-BuscaPet/BuscaPet-App) — código (backend, núcleo OpenCL, frontend, infra local)
- [`BuscaPet-Docs`](https://github.com/Equipe-BuscaPet/BuscaPet-Docs) — este repositório, com toda a documentação do projeto
- `.github` — perfil da organização

## Por que dois repositórios (código e documentação) em vez de um só

Inicialmente cada camada (backend/frontend/docs) tinha um repositório próprio,
criado antes da arquitetura técnica estar fechada. Como o projeto usa um único
`docker-compose.yml` orquestrando 5 serviços (`api`, `nucleo-opencl`,
`frontend`, `db`, `redis`), faz mais sentido ter **um monorepo de código**
(`BuscaPet-App`) — evita duplicar CI, versionamento de contrato de API e setup
de ambiente entre repositórios separados de back e front. A documentação
(escopo, planejamento, entregas de sprint) fica num repositório à parte
(`BuscaPet-Docs`) porque tem ciclo de vida e público diferente do código — cresce a
cada sprint independentemente de commits de código, e é o que se anexa/linka
nas entregas da disciplina.

`docs/schema.sql` é a única exceção: fica dentro de `BuscaPet-App` (não aqui),
porque é um artefato técnico que precisa ficar em sincronia direta com
`api/app/models/` no mesmo repositório.

## Estrutura do repositório de código (`BuscaPet-App`)

```
BuscaPet-App/
  api/                    backend FastAPI — models, config, esqueleto de rotas, testes, Alembic
  nucleo-opencl/          serviço C/OpenCL — contrato de API definido, Dockerfile com PoCL, implementação na Sprint 3
  frontend/               a estruturar por Marlon (P3 — frontend/UX)
  docs/schema.sql         modelo relacional (DDL), espelha api/app/models/
  docker-compose.yml      orquestra os 5 serviços locais (frontend, api, nucleo-opencl, db, redis)
  .github/workflows/ci.yml   lint (ruff) + testes (pytest) a cada Pull Request
  CONTEXTO.md             contexto do projeto para quem usa IA no desenvolvimento
  README.md
```

## Estrutura deste repositório (`BuscaPet-Docs`)

```
BuscaPet-Docs/
  README.md                 índice geral
  planejamento/              escopo, plano de sprints, fluxos de tela, notas técnicas
  sprints/
    sprint-1/                entrega da Sprint 1
    sprint-2/                entrega da Sprint 2 (este arquivo está aqui dentro)
  processos/                 documentos explicando decisões e processos do time (ex.: setup do Supabase)
```

## Checklist de entrega (item 7 do roteiro)

- [x] Estrutura inicial de pastas (código e documentação)
- [x] `README.md` com descrição do projeto em cada repositório
- [x] Modelos de banco, migration inicial e DDL versionados
- [x] Banco de dados criado (Supabase, migration `0001` aplicada — 19 tabelas confirmadas)
- [x] Workflow de CI configurado
- [x] Repositórios criados dentro da organização `Equipe-BuscaPet`
- [x] Primeiro commit de cada integrante em `BuscaPet-App` — Nivaldo, Kaian e Marlon já commitaram
- [ ] Primeiro commit de Marlon em `BuscaPet-Docs` — só Nivaldo e Kaian commitaram aqui até agora
- [ ] Link definitivo dos repositórios inserido no PDF final antes do envio (nota: o repositório de documentação mudou de nome ao longo do projeto — `Docs`, `Documentos` e, por fim, `BuscaPet-Docs` — e o `BuscaPet-Sprint2.docx`/`.pdf` gerado em 19/09 pode ter links para um nome anterior)
