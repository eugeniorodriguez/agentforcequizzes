# Agentforce Quiz JSON Repository

Public JSON repository for the iOS app **Agentforce Quiz**.
Repositorio publico de JSON para la app iOS **Agentforce Quiz**.

## Canonical Format

The app expects this hierarchy:

- `products[]`
- `products[].concepts[]`
- `products[].concepts[].quizzes[]`
- `products[].concepts[].quizzes[].questions[]`

Quiz shape:

- `id` (recommended suffix `-001..-999`)
- `quizNumber` (3-digit string from `001` to `999`)
- `title`
- `description`
- `difficulty`
- `certification`
- `trailheadLinks[]`
- `questions[]`

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

- `data/agentforce-sales/sales-productivity-collaboration.es.json`
- `data/agentforce-sales/sales-productivity-collaboration.en.json`

## Quiz Numbering Strategy (001..999)

Multiple quizzes are allowed under the same category (`concept`).
To avoid overwriting and keep order:

- Use `quizzes[].quizNumber` with 3 digits: `001` to `999`
- Mirror that number in `quizzes[].id` suffix (example: `sales-productivity-collaboration-001`)
- Keep question IDs local to each quiz (`q1`, `q2`, ...)

## Current Dataset

- Product: `salesforce-platform`
- Concept: `sales-productivity-collaboration`
- Quizzes:
  - `sales-productivity-collaboration-001` (`quizNumber: "001"`) - 53 preguntas
  - `sales-productivity-collaboration-002` (`quizNumber: "002"`) - 25 preguntas

## GitHub Pages

- `index.html` is the public reference page.
- Language switch (ES/EN) loads the corresponding monolingual file.

## Validation Commands

```bash
jq empty data/agentforce-sales/sales-productivity-collaboration.es.json
jq empty data/agentforce-sales/sales-productivity-collaboration.en.json
jq '.products[0].concepts[0].quizzes | map({id, quizNumber, total: (.questions|length)})' data/agentforce-sales/sales-productivity-collaboration.es.json
jq -r '.products[0].concepts[0].quizzes[].quizNumber' data/agentforce-sales/sales-productivity-collaboration.es.json | awk 'BEGIN{ok=1} {if($0 !~ /^[0-9]{3}$/){ok=0; print "Invalid quizNumber:", $0; exit 1}} END{if(ok) print "QUIZ_NUMBERS_OK"}'
```
