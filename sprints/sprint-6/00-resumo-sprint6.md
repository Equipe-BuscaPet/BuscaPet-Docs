# Sprint 6 (oficial) — aprimoramento do sistema: resumo em texto

Versão em markdown do documento da entrega (`BuscaPet-Sprint6.pdf` / `.docx`, nesta pasta, com os prints e o
diagrama de integração). Serve para leitura rápida. Se houver divergência, vale o PDF. Última revisão: 2026-10-20.

Código em [`BuscaPet-App`](https://github.com/Equipe-BuscaPet/BuscaPet-App), PR #14 (empilhado sobre o #13).

## Correções da Pré-Banca

A Pré-Banca foi **adiada** e ainda não aconteceu: não há observações formais. A Sprint tratou as pendências que a
própria equipe registrou nas Sprints 4 e 5 (doações só no modelo, selo ainda não exigido, itens "Em breve" no menu,
mapa sem página do abrigo, pontos em aberto do planejamento). As observações da banca entram quando ela acontecer.

## Terceiro módulo: Doações e apoiadores (RF-12, RF-30 a RF-35)

> abrigo verificado publica a necessidade → apoiador doa pelo mural → abrigo **confirma o recebimento** → a doação
> pontua no ranking da categoria → selo "Apoiador ativo" no perfil e nos chips do abrigo.

| Tela | Quem | O que faz |
|---|---|---|
| `/quem-apoia` | todos | Destaques do mês, ranking por categoria (mês/ano), mural com "Quero doar", "Quero apoiar" |
| `/abrigos/:id` | todos | Perfil público: contato, chave PIX (só com selo), necessidades, apoiadores, animais |
| `/apoiadores/:id` | todos | Pontos do mês e do ano, meses seguidos, selo, abrigos apoiados |
| `/doacoes` | abrigo | Publica necessidades (com selo), confirma ou recusa doações |
| `/minhas-doacoes` | apoiador | Situação das doações, contato do abrigo, cancelar enquanto aguarda |

**Regras:** selo Verificado exigido para mural, doações e chave PIX; só doação confirmada pontua; confirmação é
definitiva; necessidade vira "atendida" ao completar a quantidade; **10 pontos por doação confirmada + 10 por mês
seguido** (decide o ponto em aberto do planejamento); selo ativo = doação confirmada no mês atual ou anterior.

## Integração entre módulos

Perfil público do abrigo (animais + necessidades + apoiadores), sino único (interesse, avistamento, doação), painel
do abrigo com "o que exige ação hoje" (interessados + doações para confirmar + necessidades + animais), e navegação:
mapa e ficha do animal → perfil do abrigo → mural.

## Interface e navegação

"Quem apoia" no menu principal; itens reais no menu da conta no lugar do "Em breve"; "Ver abrigo" no mapa abre o
perfil; nome do abrigo na ficha virou link; "Quero apoiar" abre o cadastro como apoiador; cartões de necessidade com
progresso; no celular, o menu principal mostra os quatro itens sem rolar para o lado (antes "Quem apoia" ficava escondido).

## Banco e testes

Sem migration nova (tabelas da Sprint 2). 111 testes da API (15 novos de doações + 1 da demonstração), `ruff` limpo;
60/60 de ponta a ponta (8 novos); build ok; roteiro em navegador 18/18. Dados de demonstração com 4 apoiadores e
histórico de 3 meses.

## Próximos passos

Sprint 7: painel da administração com indicadores, relatórios com exportação, filtros e logs de auditoria. Mapa dos
parceiros e e-mail.
