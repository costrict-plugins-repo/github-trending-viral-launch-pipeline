# Stage 13 — Weapons Specialist

## Input
- Latest body: `stage_12_body_writer/output_v{N}.md`
- `stage_01_brand_brief/output.md` (for defensibility)

## Job
**Mutate the body in place** by scoring every line on TWO axes and rewriting any line that scores <7 on either:

1. **Invention novelty** — does this line claim something specifically only THIS product can do (or do better than competitors), as opposed to generic marketing prose?
2. **Copy intensity** — is this line sharp? Concrete? Specific? Does it use named entities, numbers, verbs? Or is it limp marketing fluff?

A line can be true and still score 3 on copy intensity. A line can be sharp and still score 3 on invention novelty.

## Method

1. Read the body.
2. For EACH line (sentence-level), assign two scores 1-10.
3. If either score < 7, rewrite the line.
4. Output the full revised body — every line should now score ≥7 on both axes.

You may DELETE lines that can't be saved. Don't preserve filler.

## Output (`stage_12_body_writer/output_post_weapons.md`)

Note: you write to the **body_writer** stage directory, not your own. The specialist chain mutates the body in place.

```markdown
# Body post-Weapons — {Product Name}

## Full revised body

{revised body, all paragraphs, every line scored ≥7 on both axes}

## What I changed (1 paragraph)
{e.g., "Removed paragraph 2 sentence 3 ('we believe in the power of automation') — scored 2 on novelty, 3 on intensity. Replaced 'the platform is fast' with 'the dashboard loads in 240ms p99 even on 50k-row datasets' — pulled the number from stage_01 feature 2."}

## Lines that survived without changes
{count + "intact paragraphs": e.g., "Paragraph 3 (demo moment) survived intact — every line scored ≥8 on both axes."}

## Word count delta
- Before: {N} words
- After: {N} words
```

## Scoring examples

### Score 9/9 (keep as-is)
> "We turned a 5-hour customer support shift into a 12-minute review queue."
- Novelty 9: specific to this product's review-queue UX, distinct from autopilot competitors
- Intensity 9: concrete number, concrete action, no fluff

### Score 4/3 (rewrite)
> "Our platform is powerful and intuitive, helping teams work faster."
- Novelty 4: every B2B SaaS says this
- Intensity 3: zero specifics, zero numbers, dead verbs

→ Rewrite: "The agent watches your queue. When it sees a refund request with order history under $200 and no flagged keywords, it drafts the reply in 8 seconds. You hit accept."

## Hard rules

- Every line in output_post_weapons.md scores ≥7 on both axes. Self-check.
- Don't add new claims that weren't in stage_01 brief or stage_07 angles. You sharpen, you don't invent.
- Preserve the 7-beat narrative structure from stage_12. Don't reorder.
- Word count can drop 10-30% (filler removal) but shouldn't grow.

## Anti-patterns

- ❌ Adding more adjectives — "powerful" → "incredibly powerful" is not a rewrite
- ❌ Adding numbers that aren't in the brief — fabrication
- ❌ Replacing a weak line with a "we believe" statement — still scores low
