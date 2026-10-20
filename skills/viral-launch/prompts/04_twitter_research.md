# Stage 04 — X/Twitter Research

## Input
- `stage_02_keywords/output.md`
- `references/search-query-templates.md` (section: Stage 4)

## Job
Find 8-12 viral product **launches** on X/Twitter in this product's category or adjacent. Extract the hook patterns, format choices, and giveaway patterns that worked.

## Tools
- `WebSearch` — run all 4 primary queries from the template, plus nitter.net fallback if X results are sparse
- `WebFetch` — fetch the top 2-3 thread URLs to extract full thread length and engagement signals
- No fabrication.

## Output (`output.md`)

```markdown
# X/Twitter Research — {Product Name}

## Queries run
- {query 1} → {N results}
- ...

## Viral launches (8-12 rows)

| # | Founder/Handle | Product | Hook (tweet 1, verbatim ≤280) | Thread len | Engagement (likes/RTs/replies) | Format | Giveaway? |
|---|---|---|---|---|---|---|---|
| 1 | @handle | {product} | "{verbatim hook}" | {N tweets} | {numbers or "high/mid/low"} | thread / single+video / single+image | yes ({type}) / no |
| 2 | ... |

## Hook pattern synthesis (1 paragraph)
{Which formats and hook patterns repeat? E.g., "7 of 10 successful launches in this category used a single tweet with embedded demo video, not a thread." Stage 8 Hook Writer needs this signal.}

## Giveaway pattern synthesis (1 paragraph)
{Of the launches with giveaways: what was given, what was the entry mechanic, what was the entry volume? Stage 10 Giveaway Writer needs this.}

## Sources
- {URL 1}
- ...
```

## Hard rules

- Tweet 1 hook must be VERBATIM (with quotes around it). Paraphrase = fail downstream.
- Engagement numbers can be approximate ("≈12k likes, ≈3k RTs") but cite the URL.
- Distinguish between "launch tweet" (product debut) and "milestone tweet" (post-launch update). Prefer launch tweets.
- If category has no viral launches → say so explicitly in synthesis. Don't pad with weak examples.
- 8-12 launches, not more.
