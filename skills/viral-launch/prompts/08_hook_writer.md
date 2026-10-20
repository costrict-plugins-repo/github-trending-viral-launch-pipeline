# Stage 08 — Hook Writer

## Input
- `stage_01_brand_brief/output.md`
- `stage_07_research_compiler/output.md`
- `references/hook-three-question-test.md`
- (iter ≥ 2 only) `stage_08_hook_writer/critique_v{N-1}.json` — address every BLOCKING and MAJOR issue

## Job
Write **5 hook candidates** for the product launch. Each candidate must:
1. Fit in ≤280 chars (X tweet 1 hard limit)
2. Pass the three-question test (what / why-now / why-never-existed)
3. Lead with one of the top-3 recommended angles from stage 7
4. Avoid every disqualifier word listed in `hook-three-question-test.md`

Five candidates from at least **two different top-3 angles** — don't write 5 variations on the same one.

## Output (`output_v{N}.md`)

```markdown
# Hook Candidates — {Product Name} (iteration {N})

## Candidate 1
**Angle used:** #{X} from stage 7
**Hook (≤280 chars):**
> {verbatim hook text — exactly what would go in tweet 1}

**Char count:** {NN} / 280

**3-question test:**
- What? — {1-line: how the reader answers from the hook alone}
- Why now? — {1-line}
- Why never existed? — {1-line + which brief/research line backs it}

## Candidate 2
{same structure, different angle preferred}

## Candidate 3
{same structure}

## Candidate 4
{same structure}

## Candidate 5
{same structure}

## Iteration notes (only if iter ≥ 2)
- Critique v{N-1} flagged {issue} → addressed in candidate #{X} by {how}
- Critique v{N-1} flagged {issue} → addressed in candidate #{Y} by {how}
- {if I disagree with a critique item, leave an HTML comment in the candidate body: `<!-- DISAGREE: <reason> -->`}
```

## Hard rules

- 5 candidates, no more, no fewer.
- Each candidate ≤280 chars (count yourself before submitting).
- At least 2 distinct angles represented across the 5.
- No disqualifier words from `hook-three-question-test.md`.
- Hook must answer all 3 test questions with information IN the hook — not in a follow-up tweet.

## Recommended hook patterns (steal liberally)

From the YouTube/X research patterns (stage 3, 4), candidates often work as:

- **Concrete-time-delta:** "We turned a 5-hour {process} into a 12-minute {process}. Live demo →"
- **Verbatim-pain echo:** "{exact reddit quote from stage 5}. We built the fix."
- **First-with-constraint:** "First {category} that {specific defensible constraint}. {1-line proof}."
- **Reverse claim:** "Everyone says {dead phrase from stage 6}. We did the opposite — and {outcome}."
- **Demo-first:** "{8-word claim}. Demo: {URL/embedded video reference}"

Mix these — don't use the same pattern across all 5.

## Anti-patterns

- ❌ "Excited to announce / Thrilled to share / Today we're launching" — feel-good openers fail the test
- ❌ "AI-powered {anything}" — dead phrase, will fail manager
- ❌ Hooks that require reading the body to make sense — fail "what is this?" question
- ❌ All 5 candidates from the same angle — manager will reject for lack of variance
