# Agentforce Quiz JSON Repository

Public JSON repository for the iOS app **Agentforce Quiz**.
Repositorio publico de JSON para la app iOS **Agentforce Quiz**.

## Canonical Format

The app expects this hierarchy:

- `products[]`
- `products[].concepts[]`
- `products[].concepts[].quizzes[]`
- `products[].concepts[].quizzes[].questions[]`

Question shape:

- `id`
- `question`
- `options` (4 strings)
- `correctIndex` (`0..3`)
- `explanation`
- `hint`

## Language Strategy (Important)

Do **not** mix Spanish and English in the same field.
No mezclar espanol e ingles en el mismo campo.

Use one monolingual JSON per language:

- `data/apex-basics.es.json`
- `data/apex-basics.en.json`

Compatibility file:

- `data/apex-basics.json` (currently EN)

## Current Dataset

- Product: `salesforce-platform`
- Concept: `apex`
- Quiz: `apex-basics`
- Questions: `100` (`q1` to `q100`)

## GitHub Pages

- `index.html` is the public reference page.
- Language switch (ES/EN) loads the corresponding monolingual file.

## Validation Commands

```bash
jq empty data/apex-basics.es.json
jq empty data/apex-basics.en.json
jq '.products[0].concepts[0].quizzes[0].questions | length' data/apex-basics.es.json
jq -r '.products[0].concepts[0].quizzes[0].questions[].id' data/apex-basics.en.json | awk 'BEGIN{ok=1} {expected="q" NR; if($0!=expected){ok=0; print "Mismatch", NR, $0, expected; exit 1}} END{if(ok) print "IDS_OK"}'
```
