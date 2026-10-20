# Artifact directory schema

All runtime artifacts live under the user's current working directory at `./launches/<slug>/`. **The plugin itself never writes inside its own install directory.**

## Slugify rules

Lowercase, alphanumerics + hyphens only, max 40 chars. Examples:
- "Cursor 2.0" → `cursor-2-0`
- "ZK-Rollup Prover" → `zk-rollup-prover`
- "Foo, Inc." → `foo-inc`

If a slug collides with an existing directory and the user does NOT say "resume", append `-v2`, `-v3`, … until free.

## Directory layout

```
launches/<slug>/
├── meta.json
├── stage_01_brand_brief/
│   ├── input.md          # what main-Claude passed (user-supplied facts)
│   └── output.md         # the structured brief
├── stage_02_keywords/
│   └── output.md
├── stage_03_youtube_research/
│   └── output.md
├── stage_04_twitter_research/
│   └── output.md
├── stage_05_reddit_research/
│   └── output.md
├── stage_06_industry_research/
│   └── output.md
├── stage_07_research_compiler/
│   └── output.md
├── stage_08_hook_writer/
│   ├── output_v1.md
│   ├── output_v2.md      # only if iter 1 failed
│   ├── output_v3.md      # only if iter 2 failed
│   └── final.md          # written by hook_manager on pass; contains ONLY the winning candidate
├── stage_09_hook_manager/
│   ├── critique_v1.json
│   ├── critique_v2.json
│   └── critique_v3.json  # only present if it ran
├── stage_10_giveaway_writer/
│   ├── output_v{N}.md
│   └── final.md
├── stage_11_giveaway_manager/
│   └── critique_v{N}.json
├── stage_12_body_writer/
│   ├── input.md          # if Mom Test triggered a kickback, list of jargon replacements appended here
│   ├── output_v1.md
│   ├── output_post_weapons.md         # written by stage 13
│   ├── output_post_controversy.md     # written by stage 14
│   ├── output_post_technical.md       # written by stage 15
│   ├── output_post_flow.md            # written by stage 16; this is what stage 17 reviews
│   ├── output_v2.md                   # only if body_manager OR mom_test kicked back
│   ├── output_post_weapons_v2.md      # specialist chain re-runs from scratch each iter
│   ├── output_post_controversy_v2.md
│   ├── output_post_technical_v2.md
│   ├── output_post_flow_v2.md
│   └── final.md          # written by body_manager on pass
├── stage_13_weapons_specialist/      # empty dir; outputs live in stage_12 (specialists mutate body in place)
├── stage_14_controversy_specialist/
├── stage_15_technical_specialist/
├── stage_16_flow_specialist/
├── stage_17_body_manager/
│   ├── critique_v1.json
│   └── critique_v2.json
├── stage_18_mom_test/
│   ├── output.md         # JARGON_FLAGS table
│   └── kickbacks.md      # only if triggered; appended to stage_12/input.md for the re-run
├── stage_19_call_supervisor/
│   ├── output_mid.md     # written between stages 11 and 12
│   └── output_late.md    # written between stages 17 and 18
├── stage_20_final_review/
│   └── output.md         # master launch document
└── stage_21_deliver/
    ├── x_thread.md
    ├── linkedin_post.md
    └── producthunt.md
```

## meta.json schema

```json
{
  "slug": "cursor-2-0",
  "product_name": "Cursor 2.0",
  "created_at": "2026-05-18T08:51:00Z",
  "last_updated": "2026-05-18T09:34:12Z",
  "current_stage": 12,
  "completed_stages": [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 19],
  "iterations": {
    "08_hook": 2,
    "10_giveaway": 1,
    "12_body": 1,
    "17_body_manager": 1,
    "18_mom_test": 0,
    "19_supervisor_mid": 1,
    "19_supervisor_late": 0
  },
  "status": "in_progress",
  "blockers": [],
  "notes": []
}
```

### Field meanings

- `current_stage` — the stage that is *about to* run, or *currently* running. On clean resume, this is where the orchestrator picks up.
- `completed_stages` — stages whose `output.md` (or `final.md` for iterated stages) is on disk and valid.
- `iterations.NN_<name>` — how many times the worker for that stage has been dispatched (so we know the budget left).
- `status` — `in_progress` | `blocked_user_input` | `done` | `aborted`
- `blockers` — list of short strings describing what's preventing progress (e.g. `"stage_19_mid_failed_drift"`)
- `notes` — free-form append-only log of orchestrator decisions, for the user's paper trail

## meta.json invariants

- `current_stage` ≥ max(`completed_stages`) — never advance back without explicit user decision
- `iterations.NN_*` ≤ 3 for worker/manager pairs, ≤ 1 for `18_mom_test`
- On `current_stage > 21`, set `status = "done"`
