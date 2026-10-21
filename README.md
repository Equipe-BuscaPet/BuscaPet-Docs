# BuscaPet-Docs

Documentação do projeto BuscaPet (UNINASSAU — Fábrica de Software, Tópicos
Avançados e Extensão IV). Código-fonte em
[`BuscaPet-App`](https://github.com/Equipe-BuscaPet/BuscaPet-App).

## Comece por aqui

- [`documentacao-final.md`](documentacao-final.md) — **documentação final consolidada**: onde está cada documento vigente, as decisões que mudaram durante o projeto e os 41 requisitos funcionais, um a um
- [`manuais/`](manuais/) — **Manual do Usuário** ([PDF](manuais/BuscaPet-Manual-do-Usuario.pdf) · [texto](manuais/manual-do-usuario.md)) e **Manual Técnico** ([PDF](manuais/BuscaPet-Manual-Tecnico.pdf) · [texto no repositório do código](https://github.com/Equipe-BuscaPet/BuscaPet-App/blob/main/docs/manual-tecnico.md))

## planejamento/

Documentos-base do projeto, anteriores às sprints:

- [`escopo-app-doacao-animais.pdf`](planejamento/escopo-app-doacao-animais.pdf) — visão geral, RF-01 a RF-41, RNFs, casos de uso
- [`plano-sprints-equipe-3.md`](planejamento/plano-sprints-equipe-3.md) — cronograma de sprints (equipe de 3, entrega final 05/12/2026)
- [`descricao-paginas-fluxos-v1.md.pdf`](planejamento/descricao-paginas-fluxos-v1.md.pdf) — telas e fluxos, tela a tela
- [`notas-tecnicas-devops-ia-v1.md.pdf`](planejamento/notas-tecnicas-devops-ia-v1.md.pdf) — decisões de arquitetura, stack e DevOps

## sprints/

Entregas de cada sprint da disciplina Fábrica de Software:

- [`sprint-1/`](sprints/sprint-1/) — definição do problema, objetivos, RFs/RNFs, backlog
- [`sprint-2/`](sprints/sprint-2/) — arquitetura, diagrama de classes, MER, modelo relacional, protótipo, banco de dados criado
- [`sprint-3/`](sprints/sprint-3/) — login e cadastro integrados ao banco, perfis de acesso, CRUD de animais, interface web e execução local (resumo em texto: [`00-resumo-sprint3.md`](sprints/sprint-3/00-resumo-sprint3.md))
- [`sprint-4/`](sprints/sprint-4/) — primeiro módulo completo (Adoção): selo Verificado, interesse de adoção, mapa de abrigos, validações e mensagens de erro (resumo em texto: [`00-resumo-sprint4.md`](sprints/sprint-4/00-resumo-sprint4.md))
- [`sprint-5/`](sprints/sprint-5/) — segundo módulo funcionando (Perdidos e achados): registro com fotos, privacidade da localização, alerta de avistamento perto, caso resolvido, testes e bugs corrigidos (resumo em texto: [`00-resumo-sprint5.md`](sprints/sprint-5/00-resumo-sprint5.md))
- [`sprint-6/`](sprints/sprint-6/) — aprimoramento do sistema: terceiro módulo (Doações e apoiadores: mural, doação com confirmação, ranking por categoria, perfis públicos), integração entre os módulos, interface e navegação (resumo em texto: [`00-resumo-sprint6.md`](sprints/sprint-6/00-resumo-sprint6.md))
- [`sprint-7/`](sprints/sprint-7/) — sistema quase completo: painel da administração com gráficos, relatórios do abrigo, pesquisas, filtros, exportação CSV e histórico de operações (resumo em texto: [`00-resumo-sprint7.md`](sprints/sprint-7/00-resumo-sprint7.md))
- [`sprint-8/`](sprints/sprint-8/) — sistema praticamente concluído: fotos dos animais, denúncia e moderação, recuperação de senha e e-mail, filtros combinados, matriz de permissões, usabilidade e revisão das regras (resumo em texto: [`00-resumo-sprint8.md`](sprints/sprint-8/00-resumo-sprint8.md))
- [`sprint-9/`](sprints/sprint-9/) — testes completos: casos de teste funcionais, de validação e de navegação, cobertura de 94%, varredura de 225 páginas, acessibilidade (WCAG 2.1 AA) e registro de bugs (resumo em texto: [`00-resumo-sprint9.md`](sprints/sprint-9/00-resumo-sprint9.md))
- [`sprint-10/`](sprints/sprint-10/) — versão Release Candidate (`1.0.0-rc.1`): sistema estável, revisão de 102 telas, banco final (MER gerado dos modelos), guia da API com as 77 rotas e README (resumo em texto: [`00-resumo-sprint10.md`](sprints/sprint-10/00-resumo-sprint10.md))
- [`sprint-11/`](sprints/sprint-11/) — preparação para entrega: manual do usuário, manual técnico, documentação final consolidada, revisão completa do código (segurança das bibliotecas, redirecionamento no login, imagens Docker) e organização do GitHub (resumo em texto: [`00-resumo-sprint11.md`](sprints/sprint-11/00-resumo-sprint11.md))

## processos/

Guias de "como fazer" para o time:

- [`comecando-a-trabalhar-no-projeto.md`](processos/comecando-a-trabalhar-no-projeto.md) — **comece por aqui**: clonar, rodar, abrir o Claude com contexto e fluxo de entrega
- [`banco-supabase-e-migrations.md`](processos/banco-supabase-e-migrations.md) — Supabase, Postgres local no Docker e migrations
- [`nucleo-opencl-spike-e-servico.md`](processos/nucleo-opencl-spike-e-servico.md) — spike OpenCL (gate da Sprint 1 interna), serviço do núcleo e primeira medição
- [`telas-reencontro-rf19-rf20.md`](processos/telas-reencontro-rf19-rf20.md) — telas de perdidos e achados (assistente em etapas) e defeitos achados no teste
- [`mapa-pwa-notificacoes-e-testes.md`](processos/mapa-pwa-notificacoes-e-testes.md) — mapa com abas e filtros, app instalável, notificações e testes de ponta a ponta

O retrato técnico atual do sistema fica no [manual técnico](https://github.com/Equipe-BuscaPet/BuscaPet-App/blob/main/docs/manual-tecnico.md) do `BuscaPet-App`; o histórico de decisões e sessões (`CONTEXTO.md`) fica na pasta local da equipe, fora do Git.
