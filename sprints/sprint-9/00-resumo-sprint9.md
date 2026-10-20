# Sprint 9 (oficial) — testes completos: resumo em texto

Versão em markdown do documento da entrega (`BuscaPet-Sprint9.pdf` / `.docx`, nesta pasta). Se houver divergência,
vale o PDF. Última revisão: 2026-10-20. Código: [`BuscaPet-App`](https://github.com/Equipe-BuscaPet/BuscaPet-App),
PR #17 (empilhado sobre o #16).

## Resultado dos testes

| Camada | Ferramenta | Resultado |
|---|---|---|
| Funcionais da API | pytest | 207 de 207 passando |
| Cobertura | pytest-cov | 94% das linhas (a CI exige no mínimo 90%) |
| Validação (CV-01 a CV-29) | pytest | 29 de 29 |
| Permissões (25 rotas × 5 perfis) | pytest | 125 de 125 |
| Ponta a ponta, computador e celular | Playwright | 85 de 85 (1 pulado de propósito: o teste só de celular no perfil de computador) |
| Varredura de navegação (5 perfis) | Playwright | 225 páginas, 0 problemas |
| Acessibilidade (24 telas, WCAG 2.1 A/AA) | axe-core | 1 defeito achado e corrigido; 0 violações graves |

Comandos: `cd api && pytest --cov=app`; `cd frontend && npm run e2e` (ou `npx playwright test varredura` /
`npx playwright test acessibilidade`).

## Casos de teste

- **Funcionais (CF-01 a CF-35):** contas e autenticação, adoção, perdidos e achados, doações, administração,
  relatórios, avisos e moderação; cada caso aponta o teste automático correspondente.
- **Validação (CV-01 a CV-29):** `api/tests/test_validacoes.py`, com código e mensagem esperados.
- **Navegação (CN-01 a CN-08):** menus por perfil, redirecionamento ao login e volta, acesso negado, página
  inexistente, sino, celular, teclado e retorno do login sem sair do site.

## Bugs

B-01 a B-07 vieram das Sprints anteriores (datas no fuso de Recife, condição de corrida na lista, alertas repetidos,
GROUP BY no PostgreSQL, nome do CSV, interesses esquecidos, aprovação que não era desfeita). **B-08, desta Sprint:**
a cor de destaque tinha contraste 4,05:1 nos links e 4,37:1 no texto dos botões (mínimo 4,5:1); passou de `#c4541a`
para `#b24c18` (4,76:1 no pior caso). Nenhum defeito aberto.

## Documentação atualizada

README (testes e comandos), `CONTEXTO.md` e `frontend/README.md` (testes das Sprints 6 a 9), `docs/schema.sql` (coluna `animais.removido_em`, migration 0004), CI com cobertura
mínima, `requirements.txt` com `pytest-cov`.

## Próximos passos

Sprint 10 (implantação: banco compartilhado, migrations e publicação), núcleo OpenCL e Sprints 11–12 (manuais,
versão final, apresentação).
