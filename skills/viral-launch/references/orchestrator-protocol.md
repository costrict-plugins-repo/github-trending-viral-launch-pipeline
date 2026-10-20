# Orchestrator dispatch protocol

This file defines the **contract** for how the `viral-launch` skill (main Claude) invokes each stage subagent.

## The dispatch wrapper

Every `Agent(subagent_type="general-purpose", prompt=...)` call must use this exact wrapper template:

```
You are the {{STAGE_NAME}} agent (stage #{{STAGE_NUM}}) in the viral-launch pipeline.

LAUNCH SLUG: {{SLUG}}
WORKING DIRECTORY: {{CWD}}
ITERATION: {{ITERATION}}   # 1, 2, or 3 for worker/manager pairs; 1 otherwise

CONTEXT FILES TO READ FIRST (in order):
{{DEPENDENCY_FILE_LIST}}

YOUR ROLE AND OUTPUT SPEC FOLLOWS:
=== BEGIN ROLE SPEC ===
{{verbatim contents of prompts/NN_<stage>.md}}
=== END ROLE SPEC ===

WRITE YOUR OUTPUT TO:
{{OUTPUT_PATH}}

Use only these tools: Read, WebSearch, WebFetch, Write, Edit, Bash (for `mkdir -p` and reading file sizes only — no destructive ops).

Return a single short paragraph (≤4 sentences) summarising what you wrote and the exact absolute path. Do not include the artifact's full content in your reply.
```

## Why hybrid (path + embedded spec)

- Dependencies are passed as **paths** so the subagent can `Read` them selectively. Cheap, lets it skim.
- The role spec is **embedded verbatim** in the prompt so the subagent does not have to discover its own job from `SKILL.md`. This keeps the orchestrator's instructions out of every subagent's context window.

## Dependency file lists per stage

| Stage | DEPENDENCY_FILE_LIST |
|---|---|
| 01 brand_brief | (user-supplied facts only, written into `input.md` by main-Claude before dispatch) |
| 02 keywords | `launches/{slug}/stage_01_brand_brief/output.md` |
| 03 youtube_research | `launches/{slug}/stage_02_keywords/output.md` |
| 04 twitter_research | `launches/{slug}/stage_02_keywords/output.md` |
| 05 reddit_research | `launches/{slug}/stage_02_keywords/output.md` |
| 06 industry_research | `launches/{slug}/stage_02_keywords/output.md` |
| 07 research_compiler | stages 03, 04, 05, 06 outputs |
| 08 hook_writer (iter N) | stage_01, stage_07, plus `stage_08_hook_writer/critique_v{N-1}.json` if N>1 |
| 09 hook_manager (iter N) | `stage_08_hook_writer/output_v{N}.md` + `references/rubric-format.md` + `references/hook-three-question-test.md` |
| 10 giveaway_writer (iter N) | stage_01, stage_08/final.md, plus critique_v{N-1} if N>1 |
| 11 giveaway_manager (iter N) | `stage_10_giveaway_writer/output_v{N}.md` + `references/rubric-format.md` |
| 12 body_writer (iter N) | stage_01, stage_07, stage_08/final, stage_10/final; plus stage_17 critique and/or stage_18 kickbacks if N>1 |
| 13 weapons_specialist | latest body output (`output_v{N}.md`) + `references/rubric-format.md` (for the dual-axis scoring rubric) |
| 14 controversy_specialist | `stage_12_body_writer/output_post_weapons.md` + stage_01 brief (defensibility) |
| 15 technical_specialist | `stage_12_body_writer/output_post_controversy.md` + stage_01 brief (facts source) |
| 16 flow_specialist | `stage_12_body_writer/output_post_technical.md` |
| 17 body_manager (iter N) | `stage_12_body_writer/output_post_flow.md` + stage_08/final + stage_10/final + `references/rubric-format.md` |
| 18 mom_test | `stage_12_body_writer/final.md` |
| 19 call_supervisor (mid)  | stage_01, stage_08/final, stage_10/final |
| 19 call_supervisor (late) | stage_01, stage_08/final, stage_10/final, stage_17/final, stage_18/output |
| 20 final_review | stage_08/final, stage_10/final, stage_17/final, stage_19/output (both passes) |
| 21 deliver | stage_20/output + `references/channel-specs.md` |

## Output path conventions

- Worker non-iterated: `stage_NN_<name>/output.md`
- Worker iterated: `stage_NN_<name>/output_v{N}.md` and on pass also `stage_NN_<name>/final.md`
- Manager: `stage_NN_<name>/critique_v{N}.json` and on pass also writes the sibling worker's `final.md` (winning artifact only)
- Specialist chain: `stage_12_body_writer/output_post_weapons.md` → `output_post_controversy.md` → `output_post_technical.md` → `output_post_flow.md`
- Mom Test: `stage_18_mom_test/output.md` (always) and `kickbacks.md` (only if triggered)
- Supervisor: `stage_19_call_supervisor/output_mid.md` and `stage_19_call_supervisor/output_late.md`

## Atomic meta.json update

After every successful `Agent` return, main-Claude updates `launches/{slug}/meta.json` in this order:

1. Read current meta.json
2. Mutate in memory: bump `current_stage`, append to `completed_stages`, increment relevant `iterations` counter, set `last_updated = now()`
3. Write meta.json back

If the agent returned an error or ambiguous summary, do **not** advance `current_stage` — surface to user instead.

## Parallel dispatch (research batch only)

For stages 3, 4, 5, 6 — main-Claude emits **one assistant message containing four `Agent` tool calls**, each with the wrapper above filled for that specific stage. The model runtime executes them in parallel. After all four return, main-Claude updates meta.json once with `completed_stages += [3,4,5,6]` and `current_stage = 7`.

No other stages parallelize.

## Worked dispatch example (stage 8, iteration 2)

```
You are the hook_writer agent (stage #8) in the viral-launch pipeline.

LAUNCH SLUG: cursor-2-0
WORKING DIRECTORY: /tmp/launch-test
ITERATION: 2

CONTEXT FILES TO READ FIRST (in order):
- /tmp/launch-test/launches/cursor-2-0/stage_01_brand_brief/output.md
- /tmp/launch-test/launches/cursor-2-0/stage_07_research_compiler/output.md
- /tmp/launch-test/launches/cursor-2-0/stage_08_hook_writer/critique_v1.json

YOUR ROLE AND OUTPUT SPEC FOLLOWS:
=== BEGIN ROLE SPEC ===
[contents of prompts/08_hook_writer.md verbatim]
=== END ROLE SPEC ===

WRITE YOUR OUTPUT TO:
/tmp/launch-test/launches/cursor-2-0/stage_08_hook_writer/output_v2.md

Use only these tools: Read, WebSearch, WebFetch, Write, Edit, Bash (for `mkdir -p` and reading file sizes only — no destructive ops).

Return a single short paragraph (≤4 sentences) summarising what you wrote and the exact absolute path. Do not include the artifact's full content in your reply.
```
