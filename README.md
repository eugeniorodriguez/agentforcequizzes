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

Use one monolingual JSON **per quiz** and per language:

- `data/agentforce-sales/sales-productivity-collaboration-001.es.json`
- `data/agentforce-sales/sales-productivity-collaboration-002.es.json`
- `data/agentforce-sales/sales-productivity-collaboration-001.en.json`
- `data/agentforce-sales/sales-productivity-collaboration-002.en.json`

## Quiz Numbering Strategy (001..999)

Multiple quizzes are allowed under the same category (`concept`).
To avoid overwriting and keep order:

- Use `quizzes[].quizNumber` with 3 digits: `001` to `999`
- Mirror that number in `quizzes[].id` suffix (example: `sales-productivity-collaboration-001`)
- Keep question IDs local to each quiz (`q1`, `q2`, ...)

## Current Dataset

- Product: `agentforce-sales`
- Concept: `sales-productivity-collaboration`
- Quizzes:
  - `sales-productivity-collaboration-001` (`quizNumber: "001"`) - 53 preguntas (ES/EN)
  - `sales-productivity-collaboration-002` (`quizNumber: "002"`) - 100 preguntas (ES), 25 preguntas (EN)

## GitHub Pages

- `index.html` is the public reference page.
- Language switch (ES/EN) loads the corresponding monolingual file.

## Validation Commands

```bash
jq empty data/agentforce-sales/sales-productivity-collaboration-001.es.json
jq empty data/agentforce-sales/sales-productivity-collaboration-002.es.json
jq empty data/agentforce-sales/sales-productivity-collaboration-001.en.json
jq empty data/agentforce-sales/sales-productivity-collaboration-002.en.json

jq '.products[0].concepts[0].quizzes[0] | {id, quizNumber, total: (.questions|length)}' data/agentforce-sales/sales-productivity-collaboration-001.es.json
jq '.products[0].concepts[0].quizzes[0] | {id, quizNumber, total: (.questions|length)}' data/agentforce-sales/sales-productivity-collaboration-002.es.json
```
