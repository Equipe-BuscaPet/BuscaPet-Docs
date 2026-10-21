# Sprint 11 (oficial) — preparação para entrega: resumo em texto

Versão em markdown do documento da entrega (`BuscaPet-Sprint11.pdf` / `.docx`, nesta pasta; o PDF traz os dois manuais
como anexos A e B). Se houver divergência, vale o PDF. Última revisão: 2026-10-21. Código:
[`BuscaPet-App`](https://github.com/Equipe-BuscaPet/BuscaPet-App), Pull Request #23 (empilhado sobre #20, #21 e #22).

## Entregas

| Entrega exigida | Onde está |
|---|---|
| Manual do Usuário | [`manuais/`](../../manuais/) (PDF e Markdown), 22 imagens, por perfil, com mensagens e perguntas frequentes |
| Manual Técnico | [`docs/manual-tecnico.md`](https://github.com/Equipe-BuscaPet/BuscaPet-App/blob/main/docs/manual-tecnico.md) no repositório do código (e o PDF em `manuais/`) |
| Documentação final consolidada | [`documentacao-final.md`](../../documentacao-final.md): documento vigente de cada assunto, 9 decisões que mudaram, RF-01 a RF-41 (37 como no escopo, 4 com mudança registrada) |
| Revisão completa do código | ruff, vulture, pip-audit, npm audit, busca de segredos no histórico e leitura módulo a módulo |
| Organização do GitHub | branches integradas apagadas (19 no App, 8 no Docs), tópicos, `.dockerignore`, README dos dois repositórios |

## O que a revisão encontrou e corrigiu

- **Redirecionamento aberto no login:** `?voltar=/\site-externo` levava a outro site (o navegador trata `/\` como
  `//`). Agora só caminhos internos; teste de ponta a ponta com 3 variantes.
- **Bibliotecas com falhas conhecidas:** python-jose → PyJWT; FastAPI 0.158 (Starlette corrigido), Pillow 13, React
  Router 7. `pip-audit`: 65 alertas em 5 pacotes → 0. `npm audit` (produção): 2 → 0.
- **`.env` iria para dentro da imagem Docker** (sem `.dockerignore`): corrigido e conferido.
- **Chave dos logins com valor de exemplo:** a API se recusa a subir num banco remoto sem um `JWT_SECRET` próprio.
- **Imagem da API em modo de desenvolvimento:** agora em produção, sem privilégios, porta `$PORT`.
- **Redis/RQ nunca usados:** removidos.

## Testes depois da revisão

API: **249 testes**, 94% de cobertura. Ponta a ponta: **91 testes** passando (computador e celular).

## Pendências e próximos passos (Sprint 12, com o líder da equipe)

Integrar os PRs #20–#23 e os da documentação; migração 0005 no Supabase; publicar em produção até 28/11 (disco
persistente para as fotos, memória para o núcleo); fotos reais na demonstração; code freeze com a tag `v1.0.0`; vídeos
e ensaio; ao final, trocar todas as senhas.
