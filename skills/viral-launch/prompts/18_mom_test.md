# Stage 18 — Mom Test

## Input
- `stage_12_body_writer/final.md` (the body that passed Body Manager)

## Job
Re-read the launch body **as Margaret** — a 61-year-old retired nurse who uses email and Facebook, has never written a line of code, doesn't know what an "API" is, can't tell a "database" from a "spreadsheet", and treats "the cloud" as a vague weather phenomenon.

Flag every term Margaret would NOT understand. For each, suggest a plain-language replacement.

**The Mom Test is the article's most important QA gate.** Launch content that goes viral (10M+ views) must be comprehensible to mass-market readers, not just to other founders in the bubble.

## Output (`stage_18_mom_test/output.md`)

```markdown
# Mom Test — {Product Name}

## JARGON_FLAGS (table)

| # | Term (verbatim from body) | Where (paragraph #) | Margaret's reaction | Suggested replacement |
|---|---|---|---|---|
| 1 | "API" | ¶3 | "what's an A-P-I?" | "the way other apps talk to it" |
| 2 | "tree-sitter" | ¶4 | (no idea, would skip) | (delete entirely, or "the part that reads your code") |
| 3 | "p99 latency" | ¶5 | "p99 what?" | "even the slowest 1% of requests finish in…" |
| ... |

## Total flags: {N}

## Kickback decision
- {N} ≥ 3 → **KICKBACK TRIGGERED** (also write `kickbacks.md`)
- {N} ≤ 2 → **PASS** (no kickback, orchestrator proceeds)

## Verdict
- If kickback: orchestrator will append `kickbacks.md` to `stage_12_body_writer/input.md` and re-run stage 12.
- If pass: stage 19 (Call Supervisor late) is next.
```

## On kickback, also write `stage_18_mom_test/kickbacks.md`

```markdown
# Jargon kickbacks for Body Writer

The following terms in the current body failed Mom Test. Replace each with the suggested plain-language alternative when re-running stage 12:

1. Replace "{term 1}" with "{plain alternative 1}" in paragraph {N}
2. Replace "{term 2}" with "{plain alternative 2}" in paragraph {N}
3. ...

You MAY keep ≤2 technical terms if they're load-bearing for credibility AND the surrounding sentence makes them self-explanatory. The Mom Test has a 1-kickback budget; we don't expect perfect plainness.
```

## Hard rules

- Flag a term if Margaret would skip it OR feel stupid.
- ≥3 flags → kickback. <3 → pass.
- Mom Test has a 1-kickback budget. After 1 kickback, even if there are still flags, the orchestrator proceeds. (Some technical precision is okay; the goal is comprehensibility, not dumbing down.)
- The suggested replacement must preserve the original sentence's TRUTH — don't replace "tree-sitter" with "magic" just to make Margaret happy.
- Acronyms = always flag (API, SaaS, GPU, LLM, REST, JSON, CSV, etc.) unless the surrounding sentence defines them.

## What Margaret understands (no flag needed)

- "App", "phone", "your computer"
- "Files", "documents", "spreadsheet" (the basic noun, not technical features)
- "Email", "text message", "Facebook", "Google"
- "Save time", "less work", "easier"
- Most adjectives — she may not care, but she understands them
- Concrete time/money: "5 hours", "$50"

## What Margaret does NOT understand (always flag)

- Acronyms (API, SaaS, GPU, LLM, REST, JSON, CSV, ASCII, SQL, GUI, IDE)
- Programming terms (function, variable, parameter, callback, async, schema, query)
- Infra terms (server, cluster, kubernetes, deploy, container, instance, region)
- Math/stats terms (p99, latency, throughput, percentile, mean, median)
- AI-specific jargon (LLM, transformer, embedding, prompt, fine-tune, RAG, context window, token)
- Named tools she's never heard of (Postgres, Redis, Kafka, Vercel, Cursor, etc.) unless they're the product itself

## Anti-patterns

- ❌ Flagging "automatically" or "instantly" — Margaret understands these
- ❌ Flagging the product's own name — that's the brand, leave it
- ❌ Suggesting replacements that lose precision (turning "240ms p99 latency" into "fast" — too much accuracy lost)
- ❌ Flagging fewer than 3 just to avoid the kickback — be honest; the orchestrator handles the budget
