# Sprint 5 (oficial) — segundo módulo funcionando: resumo em texto

Versão em markdown do documento da entrega (`BuscaPet-Sprint5.pdf` / `.docx`, nesta pasta, com os
prints e o diagrama). Serve para leitura rápida. Se houver divergência, vale o PDF. Última revisão: 2026-10-20.

Código em [`BuscaPet-App`](https://github.com/Equipe-BuscaPet/BuscaPet-App), PRs #5 a #11 e #13 (empilhados,
integrados em ordem).

## O módulo: Perdidos e achados (reencontro)

Da perda ao reencontro:

> tutor registra a perda (fotos, local, data, raio de busca) → alguém registra um avistamento → a API confere as
> perdas compatíveis e **avisa o tutor no sino** → o tutor abre o avistamento e confere → **"Encontrei meu
> animal"** → o caso sai da lista e do mapa.

| Peça | O que faz |
|---|---|
| Perdi meu animal (`/perdi`, RF-19) | Assistente em 5 etapas, 1 a 5 fotos, raio de busca. O visitante preenche e entra só para publicar |
| Encontrei um animal (`/encontrei`, RF-20) | Mesmo assistente para qualquer conta; mostra os perdidos da mesma espécie por perto |
| Lista, detalhe e mapa | Abas Perdidos / Avistados, filtros, "Perto de mim"; aba "Perdidos e avistados" no mapa |
| Alerta por proximidade (RF-25 por distância) | Notificação para o tutor quando o avistamento combina com a perda |
| Caso resolvido (RF-28) | Só o tutor autor; sai da lista e do mapa e para os alertas |
| Meus registros | Os próprios registros com o ponto exato, resolver e excluir |

## Regras de negócio

- Só tutor registra perdido; qualquer conta logada registra avistamento. Excluir: autor ou admin.
- 1 a 5 fotos por registro, tudo ou nada; fotos regravadas **sem GPS**.
- Público vê o ponto **arredondado** (perdido ~1 km, avistamento ~100 m) e nenhum dado de quem registrou.
- Alerta: mesma espécie, caso não resolvido, visto no dia do sumiço ou depois, dentro do raio do tutor; quem
  avistou não é avisado; **um alerta por tutor**, com a perda mais próxima.
- Resolvido é definitivo pela interface; registro com correspondências não pode ser excluído (409).

## Rotas novas da API (nesta Sprint)

| Rota | Quem | Função |
|---|---|---|
| `PATCH /perdidos/{id}/resolvido` | tutor autor | Marca o caso como resolvido (401/403/404/409 nos erros) |
| `POST /avistamentos` | conta logada | Agora também gera os alertas (`referencia_tipo="avistamento"`) |

As demais rotas do módulo (`/perdidos`, `/avistamentos`, `/meus`, `/{id}`, `DELETE`, `/notificacoes`) estão na seção 7 do PDF.

## Banco de dados

Sem migration nova: `animais_perdidos`, `avistamentos`, `fotos`, `notificacoes` e `buscas_salvas` já estavam
na modelagem da Sprint 2 (`0001`). Consultas SQL idênticas antes e depois de reiniciar a API e o PostgreSQL.

## Testes

95 testes da API (23 do módulo + 3 de datas) e `ruff` limpo; 52/52 testes de ponta a ponta (Playwright, banco
`buscapet_e2e`, computador e celular); build da interface ok; roteiro de demonstração em navegador 19/19 contra o
PostgreSQL de desenvolvimento; CI verde nos PRs.

## Bugs encontrados e corrigidos

| Bug | Correção |
|---|---|
| Alertas repetidos para quem tinha várias perdas | Um alerta por tutor, com a perda mais próxima (`cb4bdf1`) |
| Datas um dia antes no Postgres (e data de entrada recuando a cada edição) | Conversão para UTC antes de virar dia, `DataDoDia` (`b5632d1`) |
| Condição de corrida nas listas ao trocar aba/filtro | Só vale a resposta do último pedido, `useUltimoPedido` (`3a84fc6`) |
| Miniaturas quebradas na revisão; erro que não sumia; legenda sob o mapa | Ajustes no assistente e no detalhe (`924e3d4`) |
| Dica do iPhone quebrada palavra por palavra | Texto em parágrafo (`61d646f`) |

Sem defeito conhecido em aberto no módulo.

## Limitações e próximos passos

- Comparação por foto entra com o núcleo OpenCL (RF-21 a RF-29); contato entre quem avistou e o tutor pela
  plataforma; alerta por e-mail (RF-36).
- Sprint 6: correções da Pré-Banca, terceiro módulo (doações e apoiadores), integração e interface.

## Como este documento foi gerado

Os prints saem de um roteiro Playwright contra a API e o banco de desenvolvimento, com as contas de
demonstração; o `.docx` usa o da Sprint 4 como molde e o PDF é impresso pelo Chrome a partir do mesmo
conteúdo, com o rodapé "Sprint 5 · página N" (a capa fica sem rodapé).
