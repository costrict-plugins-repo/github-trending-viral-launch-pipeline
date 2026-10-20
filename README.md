# viral-launch-pipeline

A Claude Code plugin that replicates the 21-agent viral product launch pipeline used by some of the largest X launches. One orchestrator skill (`viral-launch`) coordinates 21 specialized subagents — research, hook writing, manager critique, weapons/controversy/technical/flow specialists, Mom Test, multi-channel delivery — to produce an X-thread + LinkedIn post + ProductHunt copy from a single user prompt.

The pipeline forces every piece of the launch to get attacked, rewritten, scored, and improved before it reaches the final version. This is the difference between AI-written launch copy that goes nowhere and copy that earns attention.

## What it produces

From one prompt (e.g. `viral launch for Cursor 2.0`), the skill writes:

- **X-thread** — 7-12 tweets (hook + body + giveaway + CTA), each within X's 280-char limit
- **LinkedIn post** — single 900-1,300 char post using a 2026 hook formula
- **ProductHunt** — tagline + description + maker's first comment + topics + gallery brief

Plus a full paper trail in `launches/<slug>/`: research notes, hook iterations, manager critiques, body specialist transformations, Mom Test report, and supervisor coherence checks.

## Installation

```bash
# In Claude Code:
/plugin marketplace add Koz-TV/viral-launch-pipeline
/plugin install viral-launch-pipeline@viral-launch-pipeline
```

Restart your Claude Code session. The `viral-launch` skill is now available.

For local development against the source tree:

```bash
/plugin marketplace add ~/Documents/Projects/skills-maker/viral-launch-pipeline
/plugin install viral-launch-pipeline@viral-launch-pipeline
```

## Usage

### Start a launch
In any directory (the skill writes artifacts to `./launches/<slug>/` relative to CWD):

```
viral launch for {Product Name}
```

The skill will:
1. Ask you for the brand-brief facts it needs (product details, ICP, founder voice, etc.)
2. Run 4 research agents in parallel (YouTube, X/Twitter, Reddit, Industry)
3. Compile novelty angles
4. Write 5 hook candidates, critique them, iterate (max 3 times)
5. Design a giveaway, critique it, iterate
6. Mid-pipeline coherence check (Call Supervisor)
7. Write the body, run 4 specialists (weapons / controversy / technical / flow), critique it, iterate
8. Run the Mom Test — flag jargon, kick back to body writer if too much (max 1 kickback)
9. Late-pipeline coherence check
10. Final polish
11. Adapt to X / LinkedIn / ProductHunt formats

You'll get short status updates at every major checkpoint. Total wall time: ~30-60 minutes depending on research breadth.

### Resume a launch
```
resume the launch for {Product Name}
```

The skill reads `./launches/<slug>/meta.json` and picks up exactly where it left off, including mid-loop iteration counters.

## Architecture

- **One orchestrator skill** (`skills/viral-launch/SKILL.md`) drives the pipeline
- **21 stage prompts** (`skills/viral-launch/prompts/01_*.md` … `21_*.md`), each self-contained
- **6 reference docs** (`skills/viral-launch/references/`):
  - `orchestrator-protocol.md` — dispatch wrapper + dependency map
  - `artifact-schema.md` — directory layout + meta.json schema
  - `rubric-format.md` — manager critique JSON schema
  - `search-query-templates.md` — queries for research stages
  - `channel-specs.md` — X/LinkedIn/PH format specs
  - `hook-three-question-test.md` — the hook QA test

The orchestrator runs every stage via `Agent(subagent_type="general-purpose", prompt=<wrapper>)`. Each subagent reads its dependencies from `launches/<slug>/`, writes its output back to the same directory, and returns a short summary. Main Claude updates `meta.json` after each stage — that's the resumability mechanism.

### The 21 stages

```
1.  Brand Brief         → 2.  Keywords      → [PARALLEL: 3-6 research] → 7.  Research Compiler
8.  Hook Writer       ↔ 9.  Hook Manager (max 3 iter)
10. Giveaway Writer   ↔ 11. Giveaway Manager (max 3 iter)
19. Call Supervisor (mid)  ← coherence check between hook + giveaway
12. Body Writer       → 13. Weapons → 14. Controversy → 15. Technical → 16. Flow → 17. Body Manager (max 3 iter)
18. Mom Test         (kicks back to 12 once if ≥3 jargon flags)
19. Call Supervisor (late) ← coherence check between hook + giveaway + body
20. Final Review
21. Deliver           → x_thread.md + linkedin_post.md + producthunt.md
```

## Why this works

Most AI-written launches are average because the AI writes the launch in one pass. This pipeline:

1. **Researches before writing.** YouTube outliers + viral X launches + verbatim Reddit pain quotes + competitor positioning maps. The hook and body inherit real audience language, not made-up marketing speak.
2. **Forces multiple drafts.** Hook Writer produces 5 candidates. Hook Manager scores them. If <8/10, writer iterates with the critique embedded as input. Repeats up to 3 times before escalating.
3. **Specialist passes on the body.** Weapons checks every line for invention novelty and copy intensity. Controversy injects one defensible contrarian beat. Technical adds credibility numbers from the brief (or flags unbackable claims). Flow polishes prose without changing facts.
4. **The Mom Test.** The article calls it "the mass-market test" — re-reading every line as a 61-year-old non-technical user. Launches that hit 10M+ views are comprehensible to anyone.
5. **Coherence supervisor.** Twice per launch: are hook + giveaway + body telling the SAME novelty story? Drift between them is the silent killer of B2B launches.

## Requirements

- Claude Code (latest)
- Built-in tools: `Agent`, `WebSearch`, `WebFetch`, `Read`, `Write`, `Edit`, `Bash`
- No external API keys
- Optional: `linkedin-skills` plugin (for LinkedIn formula library — falls back to a generic narrative if not present)

## Tips for great launches

- **Be specific in the brand brief.** "We help teams work faster" produces generic copy. "On Mondays at 9am we used to lose 87 tickets in the queue, now we process them in batches of 12" produces specific copy.
- **Provide founder voice samples.** Paste 2-3 of your past tweets or LinkedIn posts in the brief. The Body Writer will mimic the voice.
- **Don't accept iteration-3 escalations on auto-pilot.** When the manager surfaces best-of-3, READ the critiques. The honest answer is usually that the novelty thesis itself is weak — not the wording.
- **Edit the final 5%.** Claude does the heavy lifting; taste still matters. The deliverables are 95% — your job is the last polish.

## Output structure

After a complete run:

```
launches/cursor-2-0/
├── meta.json
├── stage_01_brand_brief/{input.md, output.md}
├── stage_02_keywords/output.md
├── stage_03_youtube_research/output.md
├── stage_04_twitter_research/output.md
├── stage_05_reddit_research/output.md
├── stage_06_industry_research/output.md
├── stage_07_research_compiler/output.md
├── stage_08_hook_writer/{output_v1.md, output_v2.md, final.md}
├── stage_09_hook_manager/{critique_v1.json, critique_v2.json}
├── stage_10_giveaway_writer/{output_v1.md, final.md}
├── stage_11_giveaway_manager/critique_v1.json
├── stage_12_body_writer/{output_v1.md, output_post_*.md, final.md}
├── stage_17_body_manager/critique_v1.json
├── stage_18_mom_test/output.md
├── stage_19_call_supervisor/{output_mid.md, output_late.md}
├── stage_20_final_review/output.md
└── stage_21_deliver/{x_thread.md, linkedin_post.md, producthunt.md}
```

## License

MIT. See LICENSE.

## Credit

This plugin implements the "21 specialized agents in Claude Code" architecture publicly described in a viral-launch-pipeline writeup circulating on X in 2026 (author not named in the source). The novel work here is making it portable: a single plugin a founder can install and run on their own product without rebuilding the orchestration from scratch.
