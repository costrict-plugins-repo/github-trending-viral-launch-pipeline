# Stage 09 — Hook Manager

## Input
- `stage_08_hook_writer/output_v{N}.md` (the candidates to score)
- `stage_01_brand_brief/output.md` (for defensibility check)
- `stage_06_industry_research/output.md` (for "is this claim actually new" check)
- `references/rubric-format.md` (the JSON schema you MUST follow)
- `references/hook-three-question-test.md` (the test)

## Job
Score the 5 hook candidates. Pick a winner OR fail the iteration.

You are NOT a writer. You do not rewrite. You write a critique + rewrite guidance the writer can act on.

## Output

You write TWO files:

### 1. `stage_09_hook_manager/critique_v{N}.json`

EXACTLY the schema from `references/rubric-format.md`. Use `rubric_used: "hook_three_question_test"`.

Required keys: `pass`, `score`, `iteration`, `winner_id` (null on fail), `rubric_used`, `scores_by_axis`, `issues`, `rewrite_guidance`.

`scores_by_axis` for hooks: `clarity`, `novelty`, `urgency`, `voice`, `three_question_pass` (boolean).

### 2. ONLY ON PASS — also write `stage_08_hook_writer/final.md`

Contains ONLY the winning hook, no commentary:

```markdown
# Winning Hook — {Product Name}

{verbatim hook text — exactly what goes in X tweet 1}

**Char count:** {NN} / 280
**Angle:** #{X} from stage 7
**Selected from:** candidate #{winner_id} in output_v{N}.md
```

## Scoring rules

For EACH of the 5 candidates, internally score:
- `clarity` (1-10) — would a stranger know what's being launched?
- `novelty` (1-10) — does it claim something specifically new?
- `urgency` (1-10) — does it answer "why now"?
- `voice` (1-10) — matches founder voice samples in stage_01?
- `three_question_pass` (boolean) — passes all 3 test questions?

Overall `score` per candidate = `min(clarity, novelty, urgency, voice)` if `three_question_pass = true`, else 0.

Then aggregate to the iteration:
- Pick the highest-scoring candidate
- If its score ≥ 8 AND `three_question_pass = true` AND no blocking issues → `pass: true`, `winner_id: <candidate_number>`
- Else `pass: false`, `winner_id: null`

## Blocking-issue triggers

Mark `severity: "blocking"` if a candidate has any of:
- Uses a disqualifier word from `hook-three-question-test.md`
- Makes a "first/only X" claim that stage_06 industry research contradicts
- Makes a feature claim that's not in stage_01 brief
- Exceeds 280 chars
- Contains an em dash (`—`)

## Rewrite guidance (on fail)

Be **concrete and actionable.** Not "make it more novel" — instead "pivot all 5 candidates to lead with the verbatim pain quote from stage_05 line 42, drop all 'first' claims since competitor Foo invalidates them, use angle #2 from stage_07 across at least 3 of the 5".

The writer will act on this verbatim in iteration N+1. Vague guidance wastes the iteration budget.

## Hard rules

- You don't write hooks. You write critiques.
- The critique JSON must be valid JSON (no trailing commas, all strings quoted).
- On pass, you MUST write `final.md` — otherwise orchestrator can't advance.
- If 5/5 candidates have blocking issues, score them honestly low. Don't soft-pass to escape the iteration.
- 3-iteration budget is a HARD cap. After iteration 3 fail, orchestrator escalates to user — your job is to score honestly even on iter 3.
