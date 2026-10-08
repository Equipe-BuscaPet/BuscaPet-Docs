# Banco de dados: Supabase, Docker local e migrations

## Dois bancos, dois usos

| | Supabase (compartilhado) | Postgres do Docker (local) |
|---|---|---|
| Para quê | Demonstração e dados "de verdade" do time | Desenvolvimento e testes de cada pessoa |
| Quem acessa | Quem recebeu a `DATABASE_URL` do Nivaldo | Só a sua máquina |
| Começa | Com as contas de demonstração | Vazio |

**Prefira o Docker local para desenvolver.** Use o Supabase só para validar e demonstrar.
**Nunca** commite a `DATABASE_URL`, o `.env` ou o `api/.env`: a senha do banco mora ali. A URL do
Supabase é combinada diretamente com o Nivaldo, nunca pelo repositório.

## Supabase

- Conexão pelo **Session Pooler** (porta 5432): aceita DDL (migrations) e não exige IPv6.
- O plano gratuito **pausa após ~1 semana sem uso**. Sintoma: `GET /health/db` devolve 503 dizendo que
  o projeto não foi encontrado. Solução: painel do Supabase → *Restore project*. Confira `/health/db`
  antes de qualquer demonstração.

## Docker local

```bash
cp .env.example .env                       # Windows: Copy-Item .env.example .env
docker compose up -d --build db redis api frontend
docker compose exec api alembic upgrade head
```

- `docker compose down` mantém os dados; `docker compose down -v` apaga o volume (banco zerado).
- Criar admin: `docker compose exec api python -m app.scripts.criar_admin --nome "..." --email ...`
- No Windows, o Docker Desktop precisa do WSL 2 (`wsl --install` num PowerShell como administrador,
  depois abrir o Docker Desktop). Se o Docker disser "unable to start" logo após instalar o WSL,
  feche o Docker Desktop, rode `wsl --shutdown` e abra de novo.

## Migrations (Alembic)

- Fica em `api/alembic/versions/`. Estado atual: `0001` (schema inicial) e `0002` (alinha models e banco).
- O Alembic lê a `DATABASE_URL` **da variável de ambiente**, não do `.env` (diferente da API). Num
  terminal local: `$env:DATABASE_URL = "<url>"` e depois `alembic upgrade head`.
- Mudou um model? Atualize também `docs/schema.sql`, gere uma migration
  (`alembic revision --autogenerate -m "descrição"`), **leia o arquivo gerado** antes de aplicar e
  confirme que `alembic check` fica limpo.
- Enums: já estão configurados para gravar o **valor**, não o nome (`pg_enum`). Ao criar um enum novo,
  use o mesmo mecanismo, ou o Postgres vai recusar os dados.
- Teste mudanças de banco num Postgres real. O `pytest` roda em SQLite e não pega esse tipo de erro.
