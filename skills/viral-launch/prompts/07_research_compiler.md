# Stage 07 — Research Compiler

## Input
- `stage_03_youtube_research/output.md`
- `stage_04_twitter_research/output.md`
- `stage_05_reddit_research/output.md`
- `stage_06_industry_research/output.md`
- (optional, for context) `stage_01_brand_brief/output.md`

## Job
Read all four research outputs and synthesize **5-8 novelty angles** the launch could lead with. Rank them. Recommend the top 3 to the Hook Writer.

A "novelty angle" = a one-line claim about the product that (a) is specific, (b) is defensible given the brief, (c) counter-positions against the dead phrases identified in stage 6, (d) echoes the verbatim pain language from stage 5.

## Output (`output.md`)

```markdown
# Research Compiler — Novelty Angles for {Product Name}

## All angles (5-8, ranked)

### Angle 1 — "{one-line claim}"
- **Emotional charge** (1-10): {N} — {why}
- **Defensibility** (1-10): {N} — {what proves it: brief feature X, mechanism Y, OR research finding Z (cite source: file:line)}
- **Contrarian potential** (1-10): {N} — {what dead phrase from stage 6 it counter-positions against}
- **Evidence pointers:**
  - stage_05 quote: "{verbatim}" — supports {what}
  - stage_06 competitor matrix: {Competitor X} says "{H1}" — we counter with this angle
  - stage_03 outlier hook pattern: {pattern} — our hook can use it

### Angle 2 — "{one-line claim}"
{same structure}

...

### Angle N — ...

## Recommended top-3 for Hook Writer

1. **Angle #{X}** — {one-line} — {2-sentence justification of why it's the best}
2. **Angle #{Y}** — ...
3. **Angle #{Z}** — ...

## Why I rejected the others (1 sentence each)
- Angle #{A}: {why weaker}
- ...

## Open contradictions surfaced (if any)
- {e.g. "Stage 01 brief says product is 'first AI X', but stage 06 industry research shows Competitor Foo shipped this in 2024. Hook Writer must NOT use 'first' here."}
```

## Hard rules

- **5-8 angles total.** Fewer is fine if research is thin; more dilutes.
- Each angle's claim is ≤120 chars, written as the founder would say it.
- Every defensibility line cites a specific source — stage number + line/quote.
- Emotional charge × Defensibility × Contrarian is intuitive, not formal — your overall ranking should be your honest gut feeling about which angle a Twitter audience would care about, NOT a mechanical sum.
- The recommended top-3 must include the highest-defensibility angle even if it's not the most emotional — Hook Writer needs options.

## Anti-patterns

- ❌ Angles that are restatements of features ("we have a dashboard") — features ≠ novelty
- ❌ Angles that contradict the brief — flag the contradiction in `## Open contradictions` instead
- ❌ "First X" claims with no evidence — defensibility =1, will fail in stage 9 anyway
- ❌ Refusing to rank because "they're all good" — force a ranking; downstream needs decisiveness
