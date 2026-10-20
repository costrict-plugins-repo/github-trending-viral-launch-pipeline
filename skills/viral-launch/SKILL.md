---
name: viral-launch
description: Run the full 21-agent viral launch pipeline for a product, founder announcement, or feature drop. Use when the user says "launch my product", "viral launch for X", "make me a launch thread", "write a launch announcement", "ProductHunt launch", "launch day copy". Produces X-thread + LinkedIn post + ProductHunt copy, with manager-critiqued hook, giveaway, body, and a Mom-Test pass. Resumable via "resume the launch for <product>". Keywords launch, viral, product launch, launch thread, ProductHunt, X thread, Twitter thread, hook writer, body writer, AI slop prevention.
---

# Viral Launch Pipeline — 21-stage orchestrator

You (main Claude) are the orchestrator. You do **not** write launch copy yourself. You dispatch 21 specialized subagents in sequence via the `Agent` tool, persist their artifacts to disk, and run manager-feedback loops. Reference [[1]](https://x.com/) — the article this implements states explicitly: *"Claude is not allowed to just 'write the launch.' That is how you get AI slop."*

## When to use vs. when NOT to use

**Use this skill when:**
- User asks for a product launch, viral launch, launch thread, launch announcement, ProductHunt launch, or launch day copy
- User says "resume the launch for <product>" (resumability mode)

**Do NOT use this skill for:**
- A single tweet or short post → use `linkedin-post-writer` (if installed) or write directly
- A blog post or long-form article → not this pipeline's shape
- A LinkedIn-only post (no X/PH planned) → use `linkedin-post-writer`
- "Brainstorming ideas" — this pipeline produces a specific finished launch, not options

## The 21 stages (overview)

| # | Stage | Kind | Parallel? | Max iter |
|---|---|---|---|---|
| 1 | brand_brief | single | no | 1 |
| 2 | keywords | single | no | 1 |
| 3 | youtube_research | single | **yes (batch 3-6)** | 1 |
| 4 | twitter_research | single | yes (batch 3-6) | 1 |
| 5 | reddit_research | single | yes (batch 3-6) | 1 |
| 6 | industry_research | single | yes (batch 3-6) | 1 |
| 7 | research_compiler | single | no | 1 |
| 8 | hook_writer | worker (→ 9) | no | 3 |
| 9 | hook_manager | manager (← 8) | no | — |
| 10 | giveaway_writer | worker (→ 11) | no | 3 |
| 11 | giveaway_manager | manager (← 10) | no | — |
| 19 | call_supervisor (mid) | meta | no | 1 |
| 12 | body_writer | worker (→ 17, chain 13-16) | no | 3 |
| 13 | weapons_specialist | specialist (mutates 12) | no | 1 per body iter |
| 14 | controversy_specialist | specialist | no | 1 per body iter |
| 15 | technical_specialist | specialist | no | 1 per body iter |
| 16 | flow_specialist | specialist | no | 1 per body iter |
| 17 | body_manager | manager (← 12, kicks back to 12) | no | — |
| 18 | mom_test | check (kicks back to 12) | no | 1 kickback budget |
| 19 | call_supervisor (late) | meta | no | 1 |
| 20 | final_review | single | no | 1 |
| 21 | deliver | single (multi-channel) | no | 1 |

Stage 19 (Call Supervisor) runs **twice** — once between 11 and 12 (mid), once between 17 and 18 (late). Same prompt file, different `phase` parameter in the wrapper.

## How you execute the pipeline

Follow these steps **literally**. Each "dispatch stage NN" means: build the wrapper prompt per `references/orchestrator-protocol.md` and invoke `Agent(subagent_type="general-purpose", prompt=<wrapper>)`. Update `launches/<slug>/meta.json` after every successful return.

### Step 0 — Bootstrap

When the user says something matching the trigger ("viral launch for Cursor 2.0", "make a launch thread for FooBar", "resume the launch for Cursor 2.0"):

1. **Extract product name** from the user message. Slugify per `references/artifact-schema.md` rules.
2. **Check for existing launch:**
   ```
   Bash: test -f ./launches/<slug>/meta.json && echo "exists" || echo "new"
   ```
3. **Branch:**
   - If `new`: `mkdir -p ./launches/<slug>/stage_01_brand_brief` ... (create all 21 stage subdirs), write fresh `meta.json` (current_stage=1, completed_stages=[], iterations={}, status="in_progress"). Tell the user one sentence: "Starting fresh launch for <product>. 21 stages, will check in at each manager step."
   - If `exists`:
     - Read `meta.json`
     - If user said "resume": pick up at `current_stage`. Tell user: "Resuming <product> from stage <N> (<stage_name>). <X> stages completed."
     - If user did not say "resume": ask the user (via plain text response, not a separate tool): "Found existing launch at `./launches/<slug>/` (stage <N>, status: <status>). **Resume / Start fresh (`-v2` slug) / Abort?**" — wait for their answer before proceeding.

4. **Confirm the launch parameters with user before stage 1:** product name (exact capitalization), one-line pitch, founder handle, launch date, channels (default: X + LinkedIn + ProductHunt — confirm or restrict). Write the answers to `./launches/<slug>/stage_01_brand_brief/input.md`. **Then proceed to step 1.**

### Step 1 — Stage 01 Brand Brief

Dispatch stage 01. Wrapper details in `references/orchestrator-protocol.md`. Dependency files: `stage_01_brand_brief/input.md` (which you just wrote).

If the agent returns with an `## OPEN QUESTIONS` section in its output.md, surface those to the user, get answers, append to `input.md`, re-dispatch. Loop until the brief has no open questions.

Mark stage 1 complete in `meta.json`.

### Step 2 — Stage 02 Keywords

Dispatch stage 02 with `stage_01/output.md` as dependency. One pass. Mark complete.

### Step 3 — Stages 03-06 Research (PARALLEL BATCH)

This is the only parallel batch in the entire pipeline. **In ONE assistant message, emit FOUR `Agent` tool calls** — one each for stages 03, 04, 05, 06. Each has `stage_02/output.md` as dependency.

When all four return, mark stages 3, 4, 5, 6 complete in meta.json (single update with `completed_stages += [3,4,5,6]`, `current_stage = 7`).

### Step 4 — Stage 07 Research Compiler

Dispatch with stages 03, 04, 05, 06 outputs as dependencies. One pass. Mark complete.

If the compiler's output includes `## Open contradictions surfaced` with any items, surface those to the user before proceeding — they may need to amend the brief.

### Step 5 — Stages 08 + 09 Hook Writer / Manager loop (up to 3 iter)

```
for iter in 1..3:
  dispatch stage 08 with iter=N
    deps for iter 1: stage_01, stage_07
    deps for iter ≥2: stage_01, stage_07, stage_08/critique_v{N-1}.json
  dispatch stage 09 with iter=N
    deps: stage_08/output_v{N}.md, references/rubric-format.md, references/hook-three-question-test.md, stage_01, stage_06
  Read stage_09/critique_v{N}.json
  if critique.pass == true: break
  else: continue
else:
  surface to user: best of 3 candidates (highest score) + critiques. Wait for accept / manual-edit / abort.
```

On pass, stage 09 writes `stage_08_hook_writer/final.md`. Verify the file exists before advancing. Mark stages 8 and 9 complete.

### Step 6 — Stages 10 + 11 Giveaway Writer / Manager loop

Same pattern as 8/9. Deps for stage 10 iter 1: `stage_01`, `stage_08/final.md`, `stage_04/output.md` (twitter research for giveaway patterns).

On pass, stage 11 writes `stage_10_giveaway_writer/final.md`.

### Step 7 — Stage 19 Call Supervisor (phase: mid)

Dispatch stage 19 with `phase=mid` in the wrapper. Deps: `stage_01`, `stage_08/final`, `stage_10/final`. Writes `stage_19_call_supervisor/output_mid.md`.

If verdict is FAIL:
- Surface the report to the user
- Wait for decision: re-run stage 10 / re-run stage 08 / accept drift consciously
- Act on decision

Mark stage 19 (mid) complete in meta.json (`iterations.19_supervisor_mid = 1`).

### Step 8 — Stages 12 + 13-16 + 17 Body cycle (up to 3 iter)

This is the most complex loop. Per body iteration N:

```
dispatch stage 12 (body_writer), iter=N
  deps iter 1: stage_01, stage_07, stage_08/final, stage_10/final
  deps iter ≥2: + stage_17/critique_v{N-1}.json AND/OR stage_18/kickbacks.md
dispatch stage 13 (weapons_specialist)
  deps: latest body output (output_v{N}.md)
  writes to stage_12_body_writer/output_post_weapons.md
dispatch stage 14 (controversy_specialist)
  deps: stage_12_body_writer/output_post_weapons.md, stage_01, stage_06
  writes to stage_12_body_writer/output_post_controversy.md
dispatch stage 15 (technical_specialist)
  deps: stage_12_body_writer/output_post_controversy.md, stage_01
  writes to stage_12_body_writer/output_post_technical.md
dispatch stage 16 (flow_specialist)
  deps: stage_12_body_writer/output_post_technical.md
  writes to stage_12_body_writer/output_post_flow.md
dispatch stage 17 (body_manager), iter=N
  deps: stage_12_body_writer/output_post_flow.md, stage_08/final, stage_10/final, stage_01, rubric
Read stage_17/critique_v{N}.json
if critique.pass: break
else: continue loop with N+1
```

3-iter cap. On 3-fail, surface best-of-3 (each body iter's `output_post_flow.md`) to user.

On pass, stage 17 writes `stage_12_body_writer/final.md`. Mark 12-17 complete.

### Step 9 — Stage 18 Mom Test (with 1-kickback budget)

Dispatch stage 18. Deps: `stage_12_body_writer/final.md`.

Read `stage_18_mom_test/output.md`. Count jargon flags. Read `meta.json` for `iterations.18_mom_test`.

```
if jargon_flag_count >= 3 AND iterations.18_mom_test < 1:
  meta.iterations["18_mom_test"] += 1
  # Append kickbacks.md content to stage_12_body_writer/input.md
  Read stage_18_mom_test/kickbacks.md
  Append to stage_12_body_writer/input.md (with a heading "## Mom Test kickback iteration {N}")
  # Re-run the body cycle (step 8) from stage 12 onwards
  goto step 8
else:
  # Pass (either <3 flags, or kickback budget exhausted)
  mark stage 18 complete
  proceed to step 10
```

The Mom Test is the only kickback that loops backward. It loops at most once.

### Step 10 — Stage 19 Call Supervisor (phase: late)

Dispatch stage 19 with `phase=late`. Deps: `stage_01`, `stage_08/final`, `stage_10/final`, `stage_12/final`, `stage_17/critique` (latest), `stage_18/output`. Writes `output_late.md`.

On FAIL: surface to user. Wait for decision.

Mark stage 19 (late) complete.

### Step 11 — Stage 20 Final Review

Dispatch stage 20. Deps: stage_01, stage_08/final, stage_10/final, stage_12/final, stage_19/output_mid, stage_19/output_late. Writes `stage_20_final_review/output.md` (the master launch document).

Read the output and verify all checkmarks (☑) at the bottom are true. If any is unresolved, halt and surface to user.

### Step 12 — Stage 21 Deliver

Dispatch stage 21. Deps: stage_20/output, references/channel-specs.md. The agent writes THREE files: `stage_21_deliver/x_thread.md`, `linkedin_post.md`, `producthunt.md`.

After return:
- Verify all three files exist (`Bash: ls stage_21_deliver/`)
- Read each file briefly to confirm format compliance (hook ≤280 in x_thread, char count in linkedin_post, tagline ≤60 in producthunt)
- Update meta.json: `current_stage = 22`, `status = "done"`

### Step 13 — Present deliverables to user

In a final text response (no tool call), present:

```
Launch ready for <Product Name>. 3 deliverables:

📱 X-thread → ./launches/<slug>/stage_21_deliver/x_thread.md
   <N tweets, hook NN chars>

💼 LinkedIn post → ./launches/<slug>/stage_21_deliver/linkedin_post.md
   <formula F{X}, NNN chars>

🚀 ProductHunt → ./launches/<slug>/stage_21_deliver/producthunt.md
   <tagline NN chars, description NN chars>

Full paper trail: ./launches/<slug>/

Next step: review the three files, edit the final 5% in your voice, then schedule.
```

(Use emojis here only because the user opted into a richer presentation moment; do not use them in artifact files.)

## Hard rules (NEVER violate)

1. **Never write launch copy yourself.** Every line of the launch goes through an `Agent` dispatch. Your job is the loop.
2. **Manager loops have a hard 3-iteration cap.** After iter 3 fail → surface to user with the 3 best drafts.
3. **Mom Test has a hard 1-kickback budget.** After 1 kickback → proceed regardless of remaining flags.
4. **Update meta.json after every successful Agent return.** No exceptions — this is what makes resumability work.
5. **Research batch (stages 3-6) MUST be one assistant message with 4 parallel Agent calls.** Otherwise you've spent 4× the wall time.
6. **Body kickback re-runs the ENTIRE specialist chain (12 → 13 → 14 → 15 → 16 → 17).** Specialists are deterministic transforms — they re-apply to the new body draft.
7. **Stage 19 Call Supervisor never auto-kicks-back.** Its FAIL verdicts always surface to the user.
8. **If a manager doesn't write `final.md` on pass → halt.** This is an integrity check; downstream stages depend on `final.md` paths.
9. **If a subagent returns a summary that's ambiguous about success/failure → re-dispatch with a clarification prompt, or surface to user.** Never advance on uncertain state.
10. **Never edit a manager's critique to make it pass.** Honest scores even on iter 3 — the user accepts the iteration budget; you don't get to fudge it.

## Resumability — how meta.json drives picking-up

On step 0, when meta.json exists:

```python
meta = read("./launches/<slug>/meta.json")
i = meta["current_stage"]

# If we're in the middle of a worker/manager pair, check iteration counter
# and start the loop from the right iteration
# (the worker's output_v{N}.md and critique_v{N}.json on disk tell you what's done)
```

The `iterations` map in meta.json tracks each worker's iteration count. Combined with file presence on disk (`output_v{N}.md`, `critique_v{N}.json`), you can deterministically figure out exactly which Agent call to fire next.

If meta.json says `current_stage=12, iterations.12_body=2` but only `output_v1.md` exists on disk → meta.json is ahead of disk; trust disk, fix meta.

## Resources in this skill

- **`prompts/01_brand_brief.md` … `prompts/21_deliver.md`** — the 21 role specs, one per stage. Embedded verbatim into the Agent wrapper per `orchestrator-protocol.md`.
- **`references/orchestrator-protocol.md`** — wrapper template and dependency-file map per stage
- **`references/artifact-schema.md`** — directory layout and meta.json schema
- **`references/rubric-format.md`** — manager critique JSON schema and pass rules
- **`references/search-query-templates.md`** — required queries for stages 3-6
- **`references/channel-specs.md`** — format specs for stage 21 (X / LinkedIn / ProductHunt)
- **`references/hook-three-question-test.md`** — the test stages 8 and 9 enforce

## Quick reference: dispatch wrapper template

```
You are the {STAGE_NAME} agent (stage #{STAGE_NUM}) in the viral-launch pipeline.

LAUNCH SLUG: {SLUG}
WORKING DIRECTORY: {ABSOLUTE_CWD}
ITERATION: {N}

CONTEXT FILES TO READ FIRST (in order):
{paths from orchestrator-protocol.md dependency map}

YOUR ROLE AND OUTPUT SPEC FOLLOWS:
=== BEGIN ROLE SPEC ===
{verbatim contents of prompts/NN_<stage>.md}
=== END ROLE SPEC ===

WRITE YOUR OUTPUT TO:
{path from orchestrator-protocol.md output map}

Use only these tools: Read, WebSearch, WebFetch, Write, Edit, Bash (for `mkdir -p` and reading file sizes only — no destructive ops).

Return a single short paragraph (≤4 sentences) summarising what you wrote and the exact absolute path. Do not include the artifact's full content in your reply.
```

## What to tell the user during the run

Keep updates SHORT. One sentence per major checkpoint:
- "Stage 1 (brief) done. Asking for {missing facts}." (if open questions)
- "Stages 3-6 (research) running in parallel."
- "Hook iter 1 failed (score 6, two blocking issues). Iter 2 running."
- "Hook locked. Giveaway iter 1 passed. Mid-phase Call Supervisor: PASS."
- "Body iter 1 failed at manager (demo too abstract). Iter 2 running."
- "Mom Test: 4 jargon flags. Kicking back to body — final iteration."
- "Final Review locked. Deliver running for X + LinkedIn + PH."

Don't paste artifact contents into the chat; the user can read them on disk.
