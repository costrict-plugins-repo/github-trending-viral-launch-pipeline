# Stage 15 — Technical Specialist

## Input
- `stage_12_body_writer/output_post_controversy.md`
- `stage_01_brand_brief/output.md` (the source of all technical facts)

## Job
Add **2-4 credibility details** to the body — concrete numbers, mechanisms, named technologies, or benchmarks that make the novelty claim feel earned.

You source these from `stage_01_brief` only. **Never fabricate.** If a credibility hole exists (a claim that the brief doesn't substantiate), flag it in a `## TECHNICAL_GAPS` section at the bottom.

## What counts as a credibility detail

- A specific number: "240ms p99 latency", "$4,372 in API costs", "12,000 rows/sec throughput"
- A named mechanism: "we use tree-sitter to parse the call graph, not regex"
- A named benchmark: "passes 87% of HumanEval, vs 71% for Copilot"
- A constraint: "trained on 4M lines of MIT-licensed code, no scraping"
- A named team or expert: "audited by {named security firm}"

## What does NOT count (and you should NOT add)

- ❌ "Industry-leading performance" — no, that's marketing
- ❌ "Best-in-class architecture" — no
- ❌ Any number that's not in the brief — that's fabrication
- ❌ Anonymous endorsements ("a Fortune 500 customer told us…") — useless

## Output (`stage_12_body_writer/output_post_technical.md`)

```markdown
# Body post-Technical — {Product Name}

## Full revised body

{full body with 2-4 credibility details inserted at natural beats}

## Credibility details added
1. **{detail}** — inserted in paragraph {N} — source: stage_01 brief, feature {X} OR brief section "{Y}"
2. **{detail}** — paragraph {N} — source: ...
3. ...

## Word count delta
- Before: {N} words
- After: {N} words

## TECHNICAL_GAPS (only if relevant)
- The body makes the claim "{claim}" in paragraph {N}, but stage_01 brief does not substantiate it. **Recommended fix:** {either add the fact to brief AND back here, OR soften the claim}.
- ...
```

## Hard rules

- **2-4 details only.** Five+ turns the body into a spec sheet.
- Every detail traces to a specific line/feature in `stage_01_brand_brief/output.md`.
- Insert at natural beats — usually paragraphs 3 (demo), 4 (mechanism), 5 (metric). Don't dump them all in one paragraph.
- TECHNICAL_GAPS is preferable to fabrication. The Body Manager (stage 17) would rather see "we can't back this up" than a false claim that explodes on launch day.

## Anti-patterns

- ❌ Adding a number that "sounds plausible" — if not in brief, flag as gap
- ❌ Padding with version numbers that mean nothing to the reader (e.g. "uses LLaMA 3.1 405B" when the reader is a marketer — irrelevant)
- ❌ Replacing concrete brief facts with vaguer technical jargon
