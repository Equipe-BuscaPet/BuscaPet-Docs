# Sprint 12 (oficial) — versão final do sistema: resumo em texto

Versão em markdown do documento da entrega (`BuscaPet-Sprint12.pdf` / `.docx`, nesta pasta). **Rascunho de 29/10/2026:**
o que depende do merge, do code freeze, dos vídeos e do ensaio está marcado com `[PREENCHER]` no PDF. Se houver divergência,
vale o PDF. Código: [`BuscaPet-App`](https://github.com/Equipe-BuscaPet/BuscaPet-App), Pull Request #26.

## Onde o sistema está no ar

| Peça | Hospedagem | Endereço |
|---|---|---|
| Interface | Vercel | https://busca-pet-app.vercel.app |
| API e núcleo (modo leve) | Render | https://buscapet-api.onrender.com |
| Banco e fotos | Supabase | PostgreSQL, migrações 0001 a 0006 |

O OpenCL completo, com o benchmark, roda na demonstração local (`iniciar-demo.ps1`). Na versão publicada o núcleo roda em
modo leve (mesma comparação, em C sequencial), por causa dos 512 MB do plano gratuito.

## Entregas até agora

| Item exigido | Situação |
|---|---|
| Sistema finalizado | publicado, com 11 animais, fotos reais de cães e gatos (CC BY 4.0) e a busca por foto validada (Luna em 1º, nota 0,93) |
| Correção de bugs | 6 correções registradas no documento; pytest: **255 passando, 1 pulado** (29/10) |
| Code freeze com tag | pendente: depende do merge do PR #26; passo a passo no documento (seção 4) |
| Vídeos 16:9 e 9:16 | roteiro em [`roteiro-videos.md`](roteiro-videos.md); gravação pendente |
| Ensaio | pendente |

## Pendências

Merge do PR #26 · bateria final (`pytest` e `npm run e2e`) · `1.0.0-rc.1` → `1.0.0` e tag `v1.0.0` · vídeos e ensaio ·
**depois de 05/12/2026, trocar todas as senhas** (banco, `JWT_SECRET` e contas de demonstração).
