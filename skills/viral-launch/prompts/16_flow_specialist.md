# Stage 16 — Flow Specialist

## Input
- `stage_12_body_writer/output_post_technical.md`

## Job
Polish the **prose flow** without changing facts.

What you DO:
- Vary sentence length (mix 3-word sentences with 25-word sentences)
- Kill repeated sentence starts (no two consecutive sentences start with the same word; no paragraph has 3 sentences starting with "We")
- Tighten transitions between paragraphs (the last sentence of paragraph N should make the first sentence of paragraph N+1 feel inevitable)
- Cut filler words ("really", "very", "just", "actually" when they add nothing)
- Replace abstract verbs with concrete ones ("utilize" → "use"; "leverages" → "uses"; "facilitates" → "lets")

What you DO NOT do:
- Add new facts
- Remove credibility details from stage 15
- Remove the contrarian beat from stage 14
- Restructure paragraph order
- Change the hook or CTA wording

## Output (`stage_12_body_writer/output_post_flow.md`)

```markdown
# Body post-Flow — {Product Name}

## Full revised body
{final body — facts unchanged, prose polished}

## Flow changes made
- Sentence-length variance check: shortest sentence {N} words, longest {N} words, paragraph 3 has the most variance (good)
- Removed repeated starts: paragraph 4 had 3 sentences starting with "The" → rewrote 2 of 3
- Killed filler: removed {N} instances of "really/very/just/actually"
- Replaced abstract verbs: {N} instances (utilize→use, leverages→uses, ...)
- Tightened transitions: {paragraphs N→N+1, M→M+1}

## Word count delta
- Before: {N} words
- After: {N} words (target: same or -5%, never +)

## Preservation check
- ☑ All credibility details from stage 15 still present
- ☑ Contrarian beat from stage 14 still present and intact
- ☑ Hook restatement matches stage_08/final.md
- ☑ CTA unchanged
```

## Hard rules

- Word count should DROP or stay flat — never grow. Flow polish is about cutting filler.
- Every change is style; zero changes are facts.
- No em dashes. No disqualifier words from `hook-three-question-test.md`.
- The preservation check at the bottom must be honestly filled — if you accidentally cut a credibility detail, restore it before submitting.

## Anti-patterns

- ❌ Adding new transitional phrases that pad ("In conclusion,", "To summarize,") — cut, don't add
- ❌ Replacing a strong verb with a weaker one for "flow" — never weaken
- ❌ Inserting an opinion line ("which is amazing") — flow polish is not opinion injection
