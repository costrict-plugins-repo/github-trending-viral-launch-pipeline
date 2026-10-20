# Stage 17 — Body Manager

## Input
- `stage_12_body_writer/output_post_flow.md` (the final body after all 4 specialists)
- `stage_08_hook_writer/final.md`
- `stage_10_giveaway_writer/final.md`
- `stage_01_brand_brief/output.md`
- `references/rubric-format.md`

## Job
Score the body. Pick pass or fail. On fail, the orchestrator kicks back to stage 12 (Body Writer) and the ENTIRE specialist chain (13-16) re-runs.

## Output

### 1. `stage_17_body_manager/critique_v{N}.json`

Schema from `rubric-format.md`. Use `rubric_used: "body"`.

`scores_by_axis`:
- `hook_landed` (1-10) — does paragraph 1 deliver what stage_08/final promised?
- `demo_concrete` (1-10) — is there a specific, named, scene-like demo moment?
- `novelty_defended` (1-10) — every novelty claim traces to brief or research?
- `giveaway_natural` (1-10) — does the giveaway tie-in feel earned, not bolted-on?
- `cta_clear` (1-10) — single unambiguous CTA?
- `no_ai_slop` (1-10) — zero disqualifier words / dead phrases?
- `no_brief_contradictions` (1-10) — no facts contradict stage_01?

Overall `score` = `min(all axes)`.

Pass requires `score ≥ 8` AND no blocking issues.

### 2. ON PASS — write `stage_12_body_writer/final.md`

```markdown
# Final Body — {Product Name}

{full body, exactly as in output_post_flow.md, no changes}

## Word count
{N}

## Source chain
- Hook: stage_08 final
- Giveaway: stage_10 final
- Brief: stage_01 output
- Specialist chain iterations: stage_12 body iter {N} → 13 weapons → 14 controversy → 15 technical → 16 flow
```

## Blocking-issue triggers

Mark `severity: "blocking"` if:
- Body contradicts a stage_01 brief fact (e.g. claims "first AI X" but brief says competitor Foo has it)
- Demo moment is abstract ("imagine if…") rather than a specific scene → `demo_concrete` ≤ 4
- Multiple CTAs OR no CTA → `cta_clear` ≤ 4
- TECHNICAL_GAPS from stage 15 left unresolved AND the body still makes the unsupported claim
- Disqualifier word present
- Word count <500 or >1100

## Rewrite guidance (on fail)

Be concrete. Example:
> "Paragraph 3 demo moment is too abstract ('the AI works in the background'). Rewrite as a specific named user scenario from stage_01 brief — e.g., 'a support agent on Monday morning at 9:14am sees the queue at 87 unresolved tickets…'. Keep paragraphs 1, 2, 5, 6, 7 intact (they passed). The specialist chain will re-run on the new draft."

## Hard rules

- You don't rewrite the body. You critique.
- The critique JSON is valid JSON.
- On pass, you MUST write `stage_12_body_writer/final.md` (not in your own stage directory).
- 3-iteration budget. On iter 3 fail, orchestrator escalates to user.
- If `score = 7` with no blocking issues and the body would be passable for a less-ambitious launch, still mark `pass: false` — the project standard is 8+. Surface the gap honestly.
