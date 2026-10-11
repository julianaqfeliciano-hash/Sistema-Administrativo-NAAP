# Documentação de domínio

Como as skills de engenharia devem usar a documentação de domínio deste repositório.

## Antes de explorar, leia

- `GLOSSARY.md` na raiz: a linguagem do negócio.
- `docs/adr/`: os ADRs que tocam a área do trabalho.

Se algum desses arquivos não existir, siga em frente sem apontar a falta; a skill `domain-modeling` os cria quando um termo ou uma decisão se resolve.

## Estrutura

Contexto único: um `GLOSSARY.md` e a pasta `docs/adr/` na raiz.

## Use o vocabulário do glossário

Ao nomear um conceito do domínio (título de ticket, nome de teste, tela, função), use o termo do `GLOSSARY.md` e não os sinônimos marcados em _Evitar_. Se o conceito ainda não estiver no glossário, ou a linguagem está sendo inventada (reconsidere) ou há uma lacuna real (registre para a `domain-modeling`).

## Aponte conflitos com ADRs

Se o trabalho contradiz um ADR, diga isso explicitamente em vez de passar por cima:

> _Contradiz o ADR-0002 (agenda como origem das cobranças), mas vale reabrir porque…_
