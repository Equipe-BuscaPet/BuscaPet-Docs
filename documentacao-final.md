# BuscaPet — Documentação final consolidada

Retrato da documentação do projeto na versão **1.0.0-rc.1** (Sprint 11, novembro de 2026): onde está cada documento,
o que vale hoje, o que mudou durante o desenvolvimento e como cada requisito foi atendido. Quando um documento antigo
diverge deste, **vale este** (e o código).

## 1. Onde está cada documento

| Assunto | Documento vigente | Observação |
|---|---|---|
| Requisitos (RF-01 a RF-41, RNFs) e casos de uso | [`planejamento/escopo-app-doacao-animais.pdf`](planejamento/escopo-app-doacao-animais.pdf) | as mudanças de decisão estão na seção 2 abaixo; o atendimento de cada RF, na seção 3 |
| Telas e fluxos | [`planejamento/descricao-paginas-fluxos-v1.md.pdf`](planejamento/descricao-paginas-fluxos-v1.md.pdf) e o [Manual do Usuário](manuais/manual-do-usuario.md) | o manual mostra as telas finais |
| Arquitetura | [Manual técnico, seção 1](https://github.com/Equipe-BuscaPet/BuscaPet-App/blob/main/docs/manual-tecnico.md) | a da Sprint 2 ([`sprints/sprint-2/01-arquitetura.md`](sprints/sprint-2/01-arquitetura.md)) continua valendo, sem a fila Redis/RQ |
| Diagrama de classes | [`sprints/sprint-2/02-diagrama-classes.md`](sprints/sprint-2/02-diagrama-classes.md) | as classes correspondem aos modelos; atributos novos estão no MER |
| Modelo de dados (MER) | [`docs/mer.md`](https://github.com/Equipe-BuscaPet/BuscaPet-App/blob/main/docs/mer.md) no repositório do código | **gerado dos modelos**; o da Sprint 2 ([`sprints/sprint-2/03-mer.md`](sprints/sprint-2/03-mer.md)) é histórico |
| Modelo relacional (SQL) | [`docs/schema.sql`](https://github.com/Equipe-BuscaPet/BuscaPet-App/blob/main/docs/schema.sql) | conferido contra os modelos por teste, coluna a coluna |
| API | [`docs/api.md`](https://github.com/Equipe-BuscaPet/BuscaPet-App/blob/main/docs/api.md) e o Swagger (`/docs`) | gerado do código: 77 rotas |
| Instalação, configuração, publicação e manutenção | [Manual técnico](https://github.com/Equipe-BuscaPet/BuscaPet-App/blob/main/docs/manual-tecnico.md) e o [README](https://github.com/Equipe-BuscaPet/BuscaPet-App#readme) | — |
| Uso do sistema | [Manual do Usuário](manuais/manual-do-usuario.md) ([PDF](manuais/BuscaPet-Manual-do-Usuario.pdf)) | por perfil, com o passo a passo e as mensagens |
| Componente de processamento (OpenCL) | [`nucleo-opencl/README.md`](https://github.com/Equipe-BuscaPet/BuscaPet-App/blob/main/nucleo-opencl/README.md) e o manual técnico, seção 10 | inclui o benchmark |
| Testes e casos de teste | [Sprint 9](sprints/sprint-9/00-resumo-sprint9.md) | CF, CV e CN; os números atuais estão na Sprint 11 |
| Regras de negócio revisadas | [Sprint 8](sprints/sprint-8/00-resumo-sprint8.md) e a seção 2 abaixo | — |
| Entregas de cada Sprint | [`sprints/`](sprints/) | um PDF por Sprint, mais o resumo em texto |
| Como trabalhar no projeto | [`processos/`](processos/) | atualizados na Sprint 11 |

## 2. Decisões que mudaram durante o desenvolvimento

| Tema | Previsto no início | Decisão adotada | Por quê |
|---|---|---|---|
| Aprovação de abrigo (RF-06) | o abrigo só publicava animais depois da aprovação da administração | o abrigo publica na hora; a administração concede o selo **Verificado**, exigido para o mural de necessidades, a chave PIX e as doações | Sprint 4: a aprovação travava o uso; o risco de golpe está nas doações, e é lá que o selo protege |
| Detalhe do animal (RF-15) | sobreposição (janela por cima do catálogo) | página própria, com galeria | página tem endereço próprio (dá para compartilhar o link) e funciona melhor no celular |
| Endereço manual (RF-11) | digitar o endereço quando a localização é negada | marcar o ponto no mapa | dispensa um serviço externo de busca de endereços e é mais preciso |
| Histórico em Minha conta (RF-37) | uma lista única das ações | Minha conta leva aos históricos de cada perfil (interesses, registros, doações, avisos); o abrigo e a administração têm o histórico de operações completo | cada perfil vê o histórico no contexto em que ele faz sentido |
| Monitoramento das buscas (RF-24/25) | fila de tarefas (RQ + Redis) | comparação no momento do registro, dentro da própria requisição | o núcleo responde em milissegundos e a fila só acrescentaria um serviço a manter; a configuração do Redis, nunca usada, foi removida na Sprint 11 |
| Doadores | só apoiadores (empresas) doavam | tutores também doam (itens e PIX) e têm ranking de **pessoa física**, com nome resumido por privacidade | Sprint 10, pedido da equipe: a aba "pessoa física" existia, mas não tinha como ser preenchida |
| Doação em dinheiro | só a chave PIX exibida | **meta mensal** do abrigo + o doador informa o PIX feito e o abrigo confirma; pagamento continua fora da plataforma | Sprint 10 |
| Arquivos internos | `CONTEXTO.md` e `.env.example` no repositório do código | ficam na pasta local da equipe; as linhas do `.env` estão no README | Sprint 10 |
| Bibliotecas | python-jose, React Router 6 | PyJWT, React Router 7, FastAPI 0.158, Pillow 13 | Sprint 11: auditoria de segurança das dependências |

## 3. Requisitos funcionais: como foram atendidos

Legenda: ✅ implementado como no escopo · 🔁 implementado com mudança (seção 2).

| RF | Requisito | Situação | Onde |
|---|---|---|---|
| RF-01 | Cadastro com escolha do tipo de conta | ✅ | `/cadastro` · `POST /auth/cadastro` |
| RF-02 | Login único com token | ✅ | `/login` · `POST /auth/login` (JWT) |
| RF-03 | Controle de acesso por perfil | ✅ | `exigir_tipo` em cada rota; matriz de permissões nos testes |
| RF-04 | Navegação e busca para visitante | ✅ | catálogo, mapa, perdidos e achados, quem apoia sem login |
| RF-05 | Recuperação de senha | ✅ | `/esqueci-senha` · link por e-mail, 30 min, uso único |
| RF-06 | Validação de abrigo pelo administrador | 🔁 | selo Verificado em Administração → Verificar abrigos |
| RF-07 | Mapa com a localização e pontos ao redor | ✅ | `/mapa` (OpenStreetMap + Leaflet), "Perto de mim" |
| RF-08 | Camadas: abrigos, perdidos, avistados | ✅ | abas e filtros do mapa |
| RF-09 | Lista sincronizada com o mapa, por distância | ✅ | lista ao lado do mapa |
| RF-10 | Ajuste do raio | ✅ | filtro de raio com "Perto de mim" |
| RF-11 | Endereço manual sem geolocalização | 🔁 | marcar o ponto no mapa |
| RF-12 | Perfil público do abrigo | ✅ | `/abrigos/{id}` (animais, necessidades, meta, quem apoia) |
| RF-13 | Grade responsiva, 4 por linha | ✅ | catálogo |
| RF-14 | Filtros combinados dentro do abrigo | ✅ | catálogo com o abrigo + filtros |
| RF-15 | Detalhe do animal com galeria | 🔁 | `/animais/{id}` |
| RF-16 | WhatsApp com registro prévio do interesse | ✅ | "Tenho interesse" libera o contato |
| RF-17 | Cadastro/edição de animais com fotos | ✅ | Meus animais (até 5 fotos, capa) |
| RF-18 | Status do animal e fila de interessados | ✅ | Meus animais e Interessados |
| RF-19 | Registro de perdido | ✅ | "Perdi meu animal" (5 etapas) |
| RF-20 | Registro de avistamento | ✅ | "Encontrei um animal" |
| RF-21 | Descritores visuais de toda foto | ✅ | núcleo OpenCL (`/extrair-descritor`) |
| RF-22 | Busca por similaridade com filtros prévios | ✅ | `GET /reencontro/semelhantes` (espécie, porte, cor, raio) |
| RF-23 | Resultados por semelhança, com origem, distância e data | ✅ | resultados do registro |
| RF-24 | Busca salva com monitoramento e notificação | ✅ | toda perda vira busca salva; alerta no sino |
| RF-25 | Comparação em lote contra as buscas ativas | ✅ | cada avistamento novo × perdas ativas (núcleo, modo lote) |
| RF-26 | Alerta ao abrigo com animal parecido com um perdido | ✅ | ao enviar foto de animal |
| RF-27 | Busca inversa (quem encontrou × perdidos) | ✅ | busca a partir de um avistamento |
| RF-28 | Caso resolvido | ✅ | "Encontrei meu animal" |
| RF-29 | Comparativo com e sem OpenCL | ✅ | Administração → Núcleo OpenCL |
| RF-30 | Necessidades do abrigo | ✅ | Necessidades e doações |
| RF-31 | Registro de doação | ✅ | apoiador e tutor; itens e PIX |
| RF-32 | Confirmação pelo abrigo | ✅ | Doações para confirmar |
| RF-33 | Ranking por categoria (volume e regularidade) | ✅ | Quem apoia (inclui pessoa física) |
| RF-34 | Perfil público do apoiador com selo e localização | ✅ | `/apoiadores/{id}` |
| RF-35 | Mural de necessidades em "Quem apoia" | ✅ | Quem apoia |
| RF-36 | Notificações no sistema e por e-mail | ✅ | sino, página de notificações e e-mail (SMTP) |
| RF-37 | Histórico das ações em "Minha conta" | 🔁 | Minha conta + históricos de cada perfil |
| RF-38 | Painel administrativo com moderação e indicadores | ✅ | Administração → Painel |
| RF-39 | Auditoria das ações sensíveis | ✅ | histórico de operações |
| RF-40 | Relatórios do abrigo e da administração | ✅ | Relatórios (CSV) |
| RF-41 | Denúncia de anúncio | ✅ | ficha do animal + Administração → Denúncias |

Os 41 requisitos funcionais foram atendidos: 37 como no escopo e 4 com mudança registrada.

## 4. Itens do modelo sem tela na versão 1.0

Previstos na modelagem da Sprint 2 e mantidos no banco para uma próxima versão: a equipe de vários usuários por abrigo
(`membros_equipe_abrigo`), o envio de documento comprobatório do abrigo (`abrigos.documento_comprobatorio_url`) e o tipo
de correspondência `busca_inversa` (a busca inversa funciona; só não é gravada com esse tipo). Detalhes no manual
técnico, seção 15.
