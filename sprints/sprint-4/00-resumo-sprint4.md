# Sprint 4 (oficial) — primeiro módulo completo: resumo em texto

Versão em markdown do documento da entrega (`BuscaPet-Sprint4.pdf` / `.docx`, nesta pasta, com os
prints e diagramas). Serve para leitura rápida. Se houver divergência, vale o PDF. Última revisão: 2026-10-08.

Código no `main` de [`BuscaPet-App`](https://github.com/Equipe-BuscaPet/BuscaPet-App).

## O módulo: Adoção

Do cadastro do animal pelo abrigo até a adoção em andamento:

> abrigo cadastra o animal → pessoa vê no catálogo ou no mapa → tutor registra **interesse** → abrigo
> conversa, aprova ou recusa → o animal passa para "em processo de adoção".

| Peça | O que faz |
|---|---|
| Catálogo e ficha | Busca e filtros (inclusive por abrigo), com o selo do abrigo |
| Mapa de abrigos (`/mapa`) | OpenStreetMap + Leaflet; "Abrigos perto de mim" mostra a distância e filtra por raio |
| Interesse (tutor) | Registra com mensagem opcional; só então recebe o contato do abrigo; pode desistir |
| Interessados (abrigo) | Vê o contato de cada pessoa; inicia conversa, aprova ou recusa |
| Selo Verificado (admin) | Concede o selo, retira ou suspende o abrigo |

## Rotas novas da API

| Rota | Quem | Função |
|---|---|---|
| `POST /animais/{id}/interesses` | tutor | Registra interesse (uma vez por animal, só em animal disponível) |
| `GET /interesses/meus` | tutor | Lista os seus interesses, com o contato do abrigo |
| `DELETE /interesses/{id}` | tutor | Desiste (só se aguardando ou em conversa) |
| `GET /interesses/recebidos` | abrigo | Interessados nos seus animais, com contato do tutor; filtro `status` |
| `PATCH /interesses/{id}` | abrigo | `em_conversa`, `aprovado` ou `recusado`; aprovar põe o animal em `em_processo` |
| `GET /abrigos` | público | Abrigos ativos e não suspensos; com `lat`/`lng` devolve distância e ordena; `raio_km` limita |

Situações do interesse: `aguardando` → `em_conversa` → `aprovado` ou `recusado` (aprovado e recusado são
definitivos). Cada passo cria uma `Notificacao` (ainda sem tela).

## Ajustes de planejamento, arquitetura e modelagem (e por quê)

1. **Selo Verificado no lugar da aprovação obrigatória (RF-06).** O abrigo opera ao se cadastrar;
   `pendente` = não verificado, `aprovado` = verificado, `rejeitado` = suspenso (some do catálogo e do
   mapa e não publica). Sem migration. Motivo: aprovação humana obrigatória é um gargalo; o risco de golpe
   está em exibir chave de doação e entrar no ranking, e é aí que o selo será exigido.
2. **Migration `0002`** (Sprint 3, pós-entrega): alinha modelos e banco (`alembic check` limpo).
3. **Migration `0003`:** coluna `interesses.mensagem` e restrição única `uq_interesses_animal_tutor`.
4. **Núcleo OpenCL fora das entregas oficiais de Fábrica de Software.** O roteiro oficial é o que é avaliado
   nessa disciplina; o núcleo continua obrigatório em Tópicos Avançados e é a prioridade seguinte.
5. **Mapa sem PostGIS:** distância por fórmula de haversine na API (precisão de metros, igual em SQLite e
   Postgres).
6. **Docker Compose validado** (banco, API, interface e fila).

## Mensagens de erro

Os erros de validação da API (em inglês, com nomes internos como `tutor.nome`) são traduzidos na interface
para frases como "Senha: use pelo menos 8 caracteres.", com o campo destacado. Login errado devolve sempre
"E-mail ou senha incorretos.". A tradução fica em `frontend/src/api.js` (`frase()`).

## Verificação

59 testes da API + `ruff` limpo; roteiro em navegador 17/17 contra o Postgres do Docker; fluxo no Supabase
real 15/15; migrations `0001 → 0003` em banco vazio sem divergências; dados idênticos antes e depois de
reiniciar API e banco; CI verde no `main`.

## Dificuldades e como foram resolvidas

- **Vite no Docker em Windows servia código antigo:** `usePolling` em `vite.config.js`.
- **Dependência nova (Leaflet) não aparecia no container:** o volume do `node_modules` é reaproveitado; use
  `docker compose up -d --build -V frontend` depois de puxar mudanças em `package.json`.
- **Nome do campo vinha como `tutor.nome`** (cadastro por tipo de conta): a tradução usa só o último item
  do caminho do erro.
- **Docker Desktop sem WSL 2:** `wsl --install` e, depois, `wsl --shutdown` e reabrir o Docker Desktop.

## Limitações e próximos passos

- Notificações gravadas, sem tela. Fotos dos animais ficam para o núcleo de similaridade.
- Coordenadas do abrigo são informadas no cadastro (sem geocodificação do endereço).
- Próximos: núcleo OpenCL e reencontro (RF-19 a RF-29), doações e ranking (aqui o selo passa a ser exigido),
  central de notificações e denúncia, publicação (deploy) e oficina de capacitação (Extensão IV).
