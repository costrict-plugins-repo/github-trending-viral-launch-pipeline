# Stage 06 — Industry Research

## Input
- `stage_02_keywords/output.md`
- `references/search-query-templates.md` (section: Stage 6)

## Job
Map the competitive landscape: who else is in this space, how do they position themselves, where's the empty quadrant?

## Tools
- `WebSearch` — competitor pricing, market size, "best X tools" lists
- `WebFetch` — top 3-5 competitor homepages to extract headline / sub-headline / first CTA / pricing
- No fabrication.

## Output (`output.md`)

```markdown
# Industry Research — {Product Name}

## Queries run
- {query 1} → {N results}
- ...

## Competitor matrix

| Competitor | Headline (H1, verbatim) | Sub-headline | First CTA | Entry price/mo |
|---|---|---|---|---|
| {Competitor A} | "{exact H1}" | "{sub-line}" | "{button text}" | ${X}/mo OR "contact sales" OR free |
| {Competitor B} | ... |
| {Competitor C} | ... |
| {Competitor D} | ... |
| {Competitor E} | ... |

## What every competitor says (the dead phrases)
{1 paragraph: what positioning words show up across 3+ competitors? These are the words the launch should AVOID to look different.}

## Positioning gap (1-2 paragraphs)
{Which axis is empty? E.g.: "Every competitor leads with 'AI-powered'. None of them mention the word 'control' or 'undo'. If this product gives users control/undo, that's a counter-positioning angle stage 7 should rank highly."}

## Market size signals
- {1-2 data points if found: TAM, recent funding rounds, growth rate. Mark as approximate with source URL.}

## Sources
- {URL 1} — {competitor name or article title}
- ...
```

## Hard rules

- Headlines must be VERBATIM with quotes. Paraphrasing destroys the dead-phrase analysis.
- 5 competitors is the sweet spot; 3 minimum; 7 maximum.
- "Positioning gap" must be specific and actionable — not "no one is doing X really well", but "no competitor mentions Y on their homepage".
- If pricing is gated behind a sales call ("contact us"), write that exactly — it's signal for the giveaway stage.

## Anti-patterns

- ❌ Including the product being launched as a competitor row — pointless
- ❌ Padding with adjacent-but-irrelevant tools to hit 5 — better to have 3 solid competitors
- ❌ Writing "they all do similar things" — that's the lazy version. Force yourself to find the specific empty axis.
