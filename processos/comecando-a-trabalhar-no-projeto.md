# Começando a trabalhar no projeto (com o seu Claude)

Para quem está chegando: Kaian, Marlon e qualquer pessoa nova.

## 1. Preparar a máquina

Instale: Git, Python 3.12, Node 20+ e (recomendado) Docker Desktop. Faça login no GitHub com a sua
conta e peça ao Nivaldo para ser adicionado à organização `Equipe-BuscaPet`.

## 2. Baixar os dois repositórios, lado a lado

```bash
git clone https://github.com/Equipe-BuscaPet/BuscaPet-App.git
git clone https://github.com/Equipe-BuscaPet/BuscaPet-Docs.git
```

Configure **a sua identidade** no repositório, com o e-mail vinculado ao seu GitHub, para que seus
commits apareçam como seus (o roteiro avalia a participação de cada integrante):

```bash
cd BuscaPet-App
git config user.name "Seu Nome"
git config user.email "seu-email-do-github@exemplo.com"
```

## 3. Rodar o projeto

Siga o `README.md` do `BuscaPet-App`. O caminho mais curto é o Docker (seção B); ver também
[`banco-supabase-e-migrations.md`](banco-supabase-e-migrations.md).

## 4. Abrir o Claude Code com contexto

Na pasta `BuscaPet-App`, inicie o Claude e diga:

> Leia o README.md e o docs/manual-tecnico.md. Depois leia, no repositório BuscaPet-Docs (pasta ao lado),
> o README.md e o resumo da Sprint mais recente (sprints/sprint-N/00-resumo-sprintN.md). Resuma o que entendeu
> antes de começar.

O `CONTEXTO.md` (decisões, plano interno e histórico de cada sessão) saiu do repositório na Sprint 10 e fica na
**pasta local da equipe**, que não vai para o Git. Se você a recebeu, peça também para o Claude ler o
`LEIA-PRIMEIRO-CLAUDE.md` e o `CONTEXTO.md` de lá.

Boas práticas:
- **Uma sessão por tarefa.** Termine, commite, feche. Não use uma sessão única para o projeto todo.
- Diga em qual **Sprint** está falando: a oficial da disciplina ou o plano interno (`planejamento/plano-sprints-equipe-3.md`).
- Não peça para reabrir as decisões fechadas (veja [`documentacao-final.md`](../documentacao-final.md), seção de decisões) sem um motivo novo.
- Se o Claude propuser um commit com `Co-Authored-By: Claude`, remova: a equipe decidiu não listar o
  Claude como contribuidor.
- Nunca cole senhas, a `DATABASE_URL` ou o `.env` numa conversa.

## 5. Fluxo de uma entrega

1. Crie um branch a partir do `main` (`git switch -c feature/nome-curto`).
2. Faça commits pequenos e descritivos.
3. Rode `cd api && pytest && ruff check .` e `cd frontend && npm run build`.
4. Se mexeu no banco, valide num Postgres real (Docker) e siga o fluxo de migrations.
5. Abra um Pull Request; **outra pessoa revisa**, o CI precisa estar verde.
6. Depois do merge, atualize o `BuscaPet-Docs` se a mudança afeta o que está documentado.

## 6. Divisão de papéis

- **Nivaldo (P1):** banco de dados, documentação, núcleo OpenCL.
- **Kaian (P2):** backend, integração, DevOps, apoio ao núcleo.
- **Marlon (P3):** frontend, UX, roteiro dos vídeos.

Quem tem dúvida sobre uma regra de negócio consulta primeiro `planejamento/` no `BuscaPet-Docs`
(RF-01 a RF-41), depois pergunta ao grupo.
