# Sprint 7 (oficial) — sistema quase completo: resumo em texto

Versão em markdown do documento da entrega (`BuscaPet-Sprint7.pdf` / `.docx`, nesta pasta, com os prints e os CSVs
abertos como planilha). Se houver divergência, vale o PDF. Última revisão: 2026-10-20.

Código em [`BuscaPet-App`](https://github.com/Equipe-BuscaPet/BuscaPet-App), PR #15 (empilhado sobre o #14).
Requisitos: RF-38 (painel admin), RF-39 (auditoria), RF-40 (relatórios).

| Entrega do roteiro | Como foi atendida |
|---|---|
| Dashboard | `/admin` → Painel: indicadores do período (adoções, reencontros, doações, contas novas), "agora" (abrigos para verificar, perdidos ativos, doações aguardando, necessidades), 3 gráficos mensais (6 meses) com opção de tabela, retratos de hoje e últimas ações |
| Relatórios | `/relatorios` (abrigo): interesses, adoções, tempo médio até a adoção, origem dos interessados, doações por apoiador e por item; relatório de abrigos da administração (CSV) |
| Pesquisas | Contas (nome, e-mail, nome do abrigo); histórico (quem fez e detalhes, ex.: nome do animal) |
| Filtros | Período (30 dias, 90 dias, este ano, datas) no painel, relatórios e histórico; ação; tipo de conta; situação |
| Exportação | CSV (`;` + BOM para o Excel): histórico, abrigos, interesses, doações, animais — respeita os filtros |
| Logs | `core/auditoria.py`: 27 ações sensíveis registradas na mesma transação da ação (quem, o quê, registro, quando, detalhes legíveis) |

## Decisões

- Histórico usa a tabela `logs_auditoria` da Sprint 2 (sem migration); a data das aprovações (tempo até a adoção) sai
  do histórico.
- Gráficos: uma medida por gráfico (sem dois eixos), paleta validada para daltonismo, valor no hover/foco e
  alternativa em tabela.
- Período no fuso de Recife (UTC-3), padrão 90 dias. Exportação só em CSV (justificado no PDF).

## Testes e bugs

125 testes da API (14 novos) e 66 E2E (6 novos); 13/13 rotas novas conferidas no PostgreSQL; roteiro 17/17 com 4
downloads reais. Bugs corrigidos: relatório com erro 500 no Postgres (GROUP BY com coalesce), CSV com nome genérico
(CORS sem `Content-Disposition`), detalhes do CSV com `;` e ids internos, tempo até a adoção em frações.

## Próximos passos (Sprint 8)

Recuperação de senha, denúncia, fotos dos animais e e-mail; revisão de permissões e usabilidade; núcleo OpenCL.
