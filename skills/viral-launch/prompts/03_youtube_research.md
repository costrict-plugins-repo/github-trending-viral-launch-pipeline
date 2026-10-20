# Stage 03 — YouTube Research

## Input
- `stage_02_keywords/output.md`
- `references/search-query-templates.md` (section: Stage 3)

## Job
Find 8-12 viral video **outliers** in this product's adjacent categories. Extract the hook patterns that made them outliers.

An "outlier" = a title/video that significantly outperformed the channel's median, OR a small-channel video with disproportionate views. See `references/search-query-templates.md` for the exact rule.

## Tools
- `WebSearch` — run AT LEAST the 4 queries listed in the search templates
- `WebFetch` — fetch the top 2-3 URLs (video pages) to extract titles, view counts, top comments
- No fabrication. If a query returns nothing, log "No results for: {query}".

## Output (`output.md`)

```markdown
# YouTube Research — {Product Name}

## Queries run
- {query 1} → {N results}
- {query 2} → {N results}
- ...

## Outliers (8-12 rows)

| # | Title | Channel | Subs tier | Views | Why it's an outlier | Hook pattern |
|---|---|---|---|---|---|---|
| 1 | "{exact title}" | {channel} | small/mid/big | {≈views} | {1 sentence} | number / claim / question / contrast / demo-first |
| 2 | ... |
| ... |

## Hook pattern synthesis (1 paragraph)
{Which 2-3 hook patterns repeat across the outliers? Be specific: "5 of 12 outliers use a 'I tried X for 30 days' frame". This is what stage 8 Hook Writer needs.}

## Top-comment sentiment (optional, only if WebFetch surfaced top comments)
- Most-liked comment under outlier #1: "{quote}" — what it signals
- ...

## Sources
- {URL 1} — {title}
- {URL 2} — {title}
- ...
```

## Hard rules

- 8-12 outliers, not more. Padding hurts downstream synthesis.
- Every row in the table must have a real URL in the `## Sources` list.
- "Why it's an outlier" must reference *something* about the channel's typical performance (estimate ok: "this channel usually gets 5-10k views, this hit 800k").
- If you cannot find 8 outliers, write only what you have and add a `## DEAD ENDS` section listing the queries that returned nothing — downstream Hook Writer needs to know it's working in a virgin niche.
