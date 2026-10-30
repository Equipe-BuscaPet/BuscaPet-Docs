# Sprint 12 (oficial) — versão final do sistema: resumo em texto

Versão em markdown do documento da entrega (`BuscaPet-Sprint12.pdf` / `.docx`, nesta pasta). **Atualizado em 29/10/2026,
com o code freeze feito (tag `v1.0.0`).** O que depende dos vídeos e do ensaio está marcado com `[PREENCHER]` no PDF.
Se houver divergência, vale o PDF. Código: [`BuscaPet-App`](https://github.com/Equipe-BuscaPet/BuscaPet-App),
Pull Requests #26 e #27.

## Onde o sistema está no ar

| Peça | Hospedagem | Endereço |
|---|---|---|
| Interface | Vercel | https://busca-pet-app.vercel.app |
| API e núcleo (modo leve) | Render | https://buscapet-api.onrender.com |
| Banco e fotos | Supabase | PostgreSQL, migrações 0001 a 0006 |

O OpenCL completo, com o benchmark, roda na demonstração local (`iniciar-demo.ps1`). Na versão publicada o núcleo roda em
modo leve (mesma comparação, em C sequencial), por causa dos 512 MB do plano gratuito.

## Entregas

| Item exigido | Situação |
|---|---|
| Sistema finalizado | publicado na versão `1.0.0`, com 11 animais, fotos reais de cães e gatos (CC BY 4.0) e a busca por foto validada (Luna em 1º, nota 0,93) |
| Correção de bugs | 6 correções registradas no documento; pytest: **255 passando, 1 pulado**, cobertura de 94%; E2E na CI: **90 passando, 1 pulado, 1 instável** |
| Code freeze com tag | feito: tag e release [`v1.0.0`](https://github.com/Equipe-BuscaPet/BuscaPet-App/releases/tag/v1.0.0), commit `a31053e` (`main`), 29/10/2026 |
| Vídeos 16:9 e 9:16 | roteiro em [`roteiro-videos.md`](roteiro-videos.md); gravação pendente |
| Ensaio | pendente |

## Pendências

Vídeos e ensaio · **depois de 05/12/2026, trocar todas as senhas** (banco, `JWT_SECRET` e contas de demonstração).
