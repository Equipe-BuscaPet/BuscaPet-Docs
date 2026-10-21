# Sprint 8 (oficial) — sistema praticamente concluído: resumo em texto

Versão em markdown do documento da entrega (`BuscaPet-Sprint8.pdf` / `.docx`, nesta pasta). Se houver divergência,
vale o PDF. Última revisão: 2026-10-20. Código: [`BuscaPet-App`](https://github.com/Equipe-BuscaPet/BuscaPet-App),
PR #16 (empilhado sobre o #15).

## Funcionalidades concluídas nesta Sprint

| Requisito | O que entrou |
|---|---|
| RF-17 / RF-15 | Fotos dos animais (até 5, capa, remover; sem GPS), capa no catálogo e galeria na ficha |
| RF-41 | Denúncia de anúncio; moderação: ignorar, advertir, remover anúncio, suspender abrigo (migration `0004`: `animais.removido_em`) |
| RF-05 | Recuperação de senha por e-mail (link de 30 min, uso único; mesma resposta exista ou não a conta) |
| RF-36 | Todo aviso do sino também por e-mail (SMTP configurável; sem SMTP, `.eml` na caixa de saída) |
| RF-14 | Filtros combinados: sexo, idade, castrado, vacinado, convivência |
| RF-09 / RF-11 | Ponto do mapa destaca o item da lista; posição marcada no mapa quando a localização é negada |

Com isso, todos os requisitos estão concluídos, exceto os que dependem da comparação por foto (RF-21 a RF-27, RF-29),
que seguem no núcleo OpenCL em paralelo (justificado no PDF, seção 6).

## Permissões

`tests/test_permissoes.py`: 25 rotas × 5 perfis = 125 combinações; todas passaram na primeira execução (nenhuma
brecha). Exemplos de telas barradas no PDF.

## Usabilidade

Mostrar senha, "Esqueci minha senha", título da aba por página, "Pular para o conteúdo", capa e galeria, seção de
fotos aberta depois do cadastro, "Mais filtros" com contador, posição manual no mapa, anúncio removido sinalizado.

## Regras revisadas (inconsistências corrigidas)

- Animal que sai de vez (adotado, óbito, transferido, removido) encerra os interesses que esperavam resposta.
- Animal que volta de "em processo" para "disponível" desfaz a aprovação (nunca dois aprovados).

## Testes

176 testes da API e 75/75 de ponta a ponta (o de senha lê o e-mail na caixa de saída e abre o link); migration
`0004` aplicada, desfeita e reaplicada (`alembic check` limpo); roteiro em navegador 18/18 + 3 extras.

## Próximos passos

Sprint 9 (testes completos), núcleo OpenCL e produção (aplicar a `0004` no banco compartilhado).
