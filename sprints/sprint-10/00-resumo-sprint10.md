# Sprint 10 (oficial) — versão Release Candidate: resumo em texto

Versão em markdown do documento da entrega (`BuscaPet-Sprint10.pdf` / `.docx`, nesta pasta). Se houver divergência,
vale o PDF. Última revisão: 2026-10-21. Código: [`BuscaPet-App`](https://github.com/Equipe-BuscaPet/BuscaPet-App),
Pull Requests #20, #21 e #22 (empilhados, nesta ordem).

## A versão

**`1.0.0-rc.1`**, candidata à entrega final: tudo o que será apresentado já está implementado, integrado e testado.
Módulos: contas e perfis, adoção, perdidos e achados (com a busca por semelhança no núcleo OpenCL), doações (itens e
dinheiro por PIX, meta mensal do abrigo, ranking inclusive de pessoa física) e administração. 77 rotas de API, 19
tabelas, 102 telas revisadas.

## Entregas

| Entrega exigida | Onde está |
|---|---|
| Sistema estável | 245 testes da API (94% de cobertura) e 89 testes de ponta a ponta passando; CI com 4 trabalhos a cada PR |
| Interface final | Revisão de 102 telas (5 perfis × computador e celular): 7 problemas achados e corrigidos, 0 restantes |
| Banco de dados final | 19 tabelas, 27 chaves estrangeiras, migrações 0001–0005; [`docs/mer.md`](https://github.com/Equipe-BuscaPet/BuscaPet-App/blob/main/docs/mer.md) gerado dos modelos e teste que confere o `schema.sql` |
| APIs documentadas | [`docs/api.md`](https://github.com/Equipe-BuscaPet/BuscaPet-App/blob/main/docs/api.md) gerado do código (77 rotas, quem pode usar, parâmetros, respostas, erros) e Swagger em `/docs` |
| README atualizado | [README](https://github.com/Equipe-BuscaPet/BuscaPet-App#readme) com os 9 itens pedidos |

**API própria:** a interface consome só a API do BuscaPet (FastAPI), feita pela equipe, que chama o núcleo OpenCL
(também da equipe). Serviços externos são só infraestrutura: PostgreSQL (Supabase), imagens do mapa (OpenStreetMap)
e, opcionalmente, o servidor de e-mail.

## Correções e ajustes desta Sprint

- Doação de tutor (itens e PIX) com ranking de pessoa física e **meta mensal em dinheiro do abrigo** (migração 0005).
- Revisão das telas: rolagem lateral no celular, histórico de doações separado das pendentes, página 404, textos
  defasados (cadastro de abrigo, "em construção"), contraste de link em aviso, rodapé no pé da página.
- Dois testes instáveis corrigidos (data no fuso de Recife; capa das fotos lida antes da troca).
- Aba do núcleo OpenCL explicada para quem administra; versão `1.0.0-rc.1` na API e na interface.

## Pendências e próximos passos

Migração 0005 no Supabase depois da integração dos PRs; publicação em produção até 28/11 (hospedagem do núcleo);
fotos reais na demonstração; Sprint 11 (manuais, revisão de código, GitHub); Sprint 12 (code freeze com a tag
`1.0.0`, vídeos e ensaio).
