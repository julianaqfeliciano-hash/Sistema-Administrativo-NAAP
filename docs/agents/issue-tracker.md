# Issue tracker: markdown local em `docs/`

Especificações e tickets deste repositório são arquivos markdown dentro de `docs/`, versionados junto com o código.

## Convenções

- Uma pasta por fase ou funcionalidade: `docs/<fase>/` (por exemplo, `docs/fase-1/`).
- A especificação é `docs/<fase>/spec.md`.
- Cada ticket é um arquivo próprio em `docs/<fase>/tickets/<NN>-<slug>.md`, numerado a partir de `01`; nunca um arquivo único com todos os tickets.
- Perto do topo de cada ticket ficam uma linha `Status:` (`pronto`, `em andamento` ou `feito`) e uma linha `Bloqueado por: NN, NN` quando houver dependências.
- Comentários e histórico vão no fim do arquivo, sob o título `## Comentários`.

## Quando uma skill disser "publique no issue tracker"

Crie o arquivo dentro de `docs/<fase>/`, criando a pasta se precisar.

## Quando uma skill disser "busque o ticket"

Leia o arquivo indicado; a usuária normalmente passa o caminho ou o número.
