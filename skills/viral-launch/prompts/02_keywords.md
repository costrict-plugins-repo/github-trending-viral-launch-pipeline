# Stage 02 — Keywords

## Input
- `stage_01_brand_brief/output.md`

## Job
Extract 8-15 search keywords/phrases that the research agents (stages 3-6) will use as their `WebSearch` queries. Group them into four buckets.

The keywords must be **searchable in the wild** — not internal product names. If the product is "Cursor 2.0" but normal people search for "AI code editor", use "AI code editor" as a category term.

## Output (`output.md`)

```markdown
# Keywords for {Product Name}

## category_terms (3-5)
How a stranger searches for the bucket this product is in.
- {term 1}
- {term 2}
- {term 3}

## competitor_terms (2-4)
Names of competing products + the phrase "alternative to {competitor}".
- {Competitor A}
- {Competitor B}
- alternative to {Competitor A}

## pain_terms (3-5)
The verbatim language a frustrated user types when they have the problem this product solves. Lift from `stage_01` brief or guess; later stage 5 will verify with Reddit.
- "{exact phrasing of pain 1}"
- "{exact phrasing of pain 2}"
- "{a "I hate X" style phrase}"

## outcome_terms (2-4)
The job-to-be-done outcome the user wants. Concrete verbs, not benefits.
- "{verb-phrase outcome 1}"
- "{verb-phrase outcome 2}"
```

## Rules

- **8-15 keywords total across all 4 buckets.** Under 8 starves research; over 15 dilutes.
- **Lowercase** unless the term is a proper noun.
- **Use quotes** around multi-word phrases that should be searched as a unit.
- **No marketing words** — "intelligent X", "AI-powered Y" — those don't appear in search history.
- If brief lists a niche term the audience uses (jargon ok at this stage if it's how the audience really talks), include it.

## Anti-patterns

- ❌ `competitor_terms: ["Notion", "ClickUp", "Asana", "Monday", "Linear", "Jira", "Trello"]` — pick 2-4 actual competitors, not a list of every tool in the space
- ❌ `pain_terms: ["productivity issues"]` — too abstract; nobody types this
- ❌ Reusing the product's own name in `category_terms` — that's not how strangers search
