# Sistema Administrativo NAAP

Sistema interno de gestão da clínica NAAP (Núcleo de Atendimento e Avaliação Psicológica, Fortaleza): agenda, cobranças, repasses, despesas e indicadores. Quem usa são as quatro sócias e a secretária. A linguagem do negócio, das telas e dos testes é português do Brasil.

## Antes de mexer

- Use exatamente os termos de `GLOSSARY.md` em telas, código, testes e tickets; os termos listados em _Evitar_ não aparecem.
- Leia os ADRs de `docs/adr/` que tocam a área do trabalho.
- A fase em andamento está especificada em `docs/fase-1/spec.md`.
- O sistema não guarda conteúdo clínico (ADR-0001).
- Dados reais de pacientes nunca entram no repositório: testes, sementes e demonstração usam dados inventados.

## Processo

O trabalho segue o fluxo das skills de Matt Pocock (github.com/mattpocock/skills): grill-with-docs → to-spec → to-tickets → implement (com tdd) → code-review. Se as skills não estiverem instaladas, clone o repositório e leia o `SKILL.md` de cada uma.

## Agent skills

### Issue tracker

Especificações e tickets são arquivos markdown dentro de `docs/<fase>/`. See `docs/agents/issue-tracker.md`.

### Domain docs

Single-context: `GLOSSARY.md` e `docs/adr/` na raiz. See `docs/agents/domain.md`.
