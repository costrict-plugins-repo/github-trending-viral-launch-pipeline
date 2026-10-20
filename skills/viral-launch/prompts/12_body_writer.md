# Stage 12 — Body Writer

## Input
- `stage_01_brand_brief/output.md`
- `stage_07_research_compiler/output.md`
- `stage_08_hook_writer/final.md`
- `stage_10_giveaway_writer/final.md`
- (iter ≥ 2 only) `stage_17_body_manager/critique_v{N-1}.json` AND/OR `stage_18_mom_test/kickbacks.md`
- (iter ≥ 2 only) `stage_12_body_writer/input.md` may have appended jargon replacement instructions

## Job
Turn the chosen hook into a **demo-driven narrative body** of 600-1000 words. Channel-agnostic — stage 21 will adapt to X/LinkedIn/PH.

Your body will then be mutated in-place by the specialist chain (13 Weapons → 14 Controversy → 15 Technical → 16 Flow). Don't try to do their jobs. Write the strong base, they polish.

## The required body structure (in this order)

1. **Hook restatement / opener** (1-2 sentences) — echo the winning hook, then a beat that earns the reader's next 30 seconds
2. **Problem framing** (2-3 sentences) — verbatim pain from stage_05 if available, then "and here's why nothing has solved it"
3. **The demo moment** (3-5 sentences) — ONE specific, named, scene-like moment of the product in action. Concrete user, concrete action, concrete output. NOT "imagine if…".
4. **What changed under the hood** (2-3 sentences) — the mechanism. WHY this works. From stage_01 brief features.
5. **Before/after metric** (1-2 sentences) — exact number from brief, NOT a vague "10x". E.g. "5 hours → 12 minutes" or "$4,372 → $284".
6. **Giveaway tie-in** (1-2 sentences) — natural beat to mention the giveaway. Should feel like a continuation, not a bolt-on.
7. **CTA** (1 sentence) — single action, low friction. "Try it: {URL}" or "Reply with {X} to enter."

## Output (`output_v{N}.md`)

```markdown
# Body — {Product Name} (iteration {N})

## Full body (600-1000 words, channel-agnostic)

{paragraph 1 — hook opener}

{paragraph 2 — problem framing}

{paragraph 3 — demo moment}

{paragraph 4 — what changed under the hood}

{paragraph 5 — before/after metric}

{paragraph 6 — giveaway tie-in}

{paragraph 7 — CTA}

## Word count
{NNN}

## Beats covered (self-check)
- ☑ Hook restated / opener
- ☑ Problem framing
- ☑ Demo moment (specific, named, concrete)
- ☑ Mechanism explained
- ☑ Before/after metric (real number, not vague)
- ☑ Giveaway tie-in
- ☑ Single clear CTA

## Sources used
- stage_01 brief: features {X, Y}
- stage_05 reddit: quote "{verbatim}" used in problem framing
- stage_07 angle: #{X} ("{claim}")

## Iteration notes (if iter ≥ 2)
- Critique v{N-1} flagged "demo moment too abstract" → rewrote paragraph 3 with the {X} user scenario from brief
- Mom Test kicked back {N} jargon terms → replaced "{tech term}" with "{plain alternative}" in paragraphs {Y, Z}
```

## Hard rules

- 600-1000 words. Under = thin. Over = will not survive channel adaptation in stage 21.
- ONE demo moment, not three. Specific beats specific.
- Every novelty claim traces to stage_01 brief or stage_07 angles. If you need a claim that isn't in either, flag with `<!-- NEEDS_BRIEF_FACT: {what} -->` HTML comment.
- No em dashes. No disqualifier words from `hook-three-question-test.md`.
- The giveaway tie-in must reference the giveaway by what it IS, not just "we're running a giveaway".

## Anti-patterns

- ❌ "Imagine if you could…" → not a demo moment, just a feature wish
- ❌ "10x faster" / "incredibly fast" → vague metric, fail in stage 13
- ❌ "We built this because…" funding-style backstory → cut, irrelevant to the reader
- ❌ Bullet lists of features → kills narrative; the body is a story, not a spec sheet
- ❌ Multiple CTAs → reader picks none; one CTA only
