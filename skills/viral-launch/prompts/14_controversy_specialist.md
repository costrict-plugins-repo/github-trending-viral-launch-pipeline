# Stage 14 — Controversy Specialist

## Input
- `stage_12_body_writer/output_post_weapons.md`
- `stage_01_brand_brief/output.md` (defensibility check)
- `stage_06_industry_research/output.md` (the dead phrases competitors use)

## Job
Inject **ONE contrarian edge** into the body — a paragraph or sentence that takes a defensible-but-uncomfortable position that the competitor wouldn't say.

ONE. Not three. Multiple contrarian beats dilute the bite.

The contrarian beat must be:
1. **Defensible** by stage_01 brief facts — if challenged, founder can back it up
2. **Counter-positioning** against the dead phrases from stage_06 — saying out loud what competitors avoid
3. **Embedded in the narrative** — not a standalone "hot take", but a beat that strengthens the demo or mechanism

## Method

1. Read the body and the stage_06 dead-phrase list.
2. Pick ONE place where a contrarian beat fits naturally (usually paragraph 4 "what changed under the hood" or paragraph 5 "metric").
3. Write the contrarian beat.
4. Output the full revised body with the beat embedded.

## Output (`stage_12_body_writer/output_post_controversy.md`)

```markdown
# Body post-Controversy — {Product Name}

## Full revised body

{full body with ONE contrarian beat embedded}

## The contrarian beat
**Where:** paragraph {N}, sentence {M}
**What it says:** "{verbatim line(s)}"
**Why it's contrarian:** {1 sentence — which competitor dead phrase from stage_06 it counter-positions against}
**Defensibility:** {1 sentence — what brief fact or mechanism backs it}

## What I changed (1 paragraph)
{description of the embedding decision}

## Word count delta
- Before: {N} words
- After: {N} words
```

## Examples of contrarian beats that work

- **"Most {category} tools are designed to be autonomous. We made ours deliberately interruptible. The agent stops every 30 seconds and asks you a question. That's the whole point."**
  → counter-positions against the AI-autopilot trend
- **"We don't have a mobile app. We never will. {Product} is a tool for the 2 hours a day you're at your desk doing work that matters."**
  → counter-positions against "build for every platform" instinct
- **"The first version cost us $84 in API credits per user. We kept the architecture anyway, because the alternative was the same dumb autocomplete every competitor ships."**
  → counter-positions against "AI must be cheap"

## Examples of bad contrarian beats (don't write these)

- ❌ "Unlike our competitors, we care about quality." — not contrarian, every competitor claims this too
- ❌ "We think AI is overhyped." — too abstract, no defensible specific
- ❌ "Disclaimer: this product is not for everyone." — empty edginess

## Hard rules

- Exactly ONE contrarian beat. Mark it clearly in the output for stage 15 to see.
- Defensibility traces to stage_01 brief — not "we believe", but "we did X because Y".
- The beat lives INSIDE a narrative paragraph; it's not a standalone callout.
- No insult of named competitors. Counter-position against the *pattern*, not the company.
- If you can't find a defensible contrarian beat, write `## NO CONTRARIAN BEAT POSSIBLE` and explain why (e.g. "product genuinely doesn't disagree with the market consensus on anything"). Stage 17 manager will evaluate.

## Anti-patterns

- ❌ Adding multiple contrarian beats — dilutes
- ❌ Naming competitors negatively ("unlike Foo, we…") — looks small
- ❌ Manufacturing controversy by overclaiming — backfires in QA replies
