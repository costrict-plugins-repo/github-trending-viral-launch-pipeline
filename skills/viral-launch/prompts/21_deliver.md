# Stage 21 — Deliver

## Input
- `stage_20_final_review/output.md` (the master launch document)
- `references/channel-specs.md` (THE format spec for each channel — read this carefully)
- (optional, if file exists) `~/.claude/plugins/marketplaces/linkedin-skills/skills/linkedin-post-writer/references/hook-formulas.md` (LinkedIn formula library; check via `Bash test -f` first)

## Job
Adapt the master launch document into **three channel-specific deliverables**:
1. `x_thread.md` — X-thread (hook + 7-12 body tweets + giveaway + CTA)
2. `linkedin_post.md` — Single LinkedIn post (900-1300 chars)
3. `producthunt.md` — ProductHunt tagline + description + maker's first comment

Each must hit the channel's exact format constraints from `references/channel-specs.md`. The novelty angle must be the SAME across all three — only the format changes.

## Method

1. **Read `references/channel-specs.md`** in full. The constraints there are exhaustive.
2. **Check for the linkedin-skills hook-formulas reference:**
   ```bash
   test -f ~/.claude/plugins/marketplaces/linkedin-skills/skills/linkedin-post-writer/references/hook-formulas.md && echo "exists" || echo "missing"
   ```
   If exists, read it and pick a formula appropriate to the launch type. If missing, use `generic-launch-narrative` fallback (also documented in channel-specs.md).
3. **Adapt for each channel separately.** Do NOT generate all three at once and then "tweak" — each format has different muscle.
4. **Run the cross-channel consistency check** documented at the bottom of `channel-specs.md` before finalizing. If a check fails, fix and re-verify.

## Output

You write THREE files (paths exact):

### `stage_21_deliver/x_thread.md`
Format from `channel-specs.md` "Channel 1: X-thread" — verbatim template. Fill every section.

### `stage_21_deliver/linkedin_post.md`
Format from `channel-specs.md` "Channel 2: LinkedIn post" — verbatim template. Choose formula (F1/F7/F10/generic).

### `stage_21_deliver/producthunt.md`
Format from `channel-specs.md` "Channel 3: ProductHunt" — verbatim template.

## Hard rules

- **All three files must exist** before you return success. Missing one = task failure.
- **Each file passes its own format checks** (char counts, tweet counts, no em dashes, etc.). Self-check before writing.
- **The hook claim is consistent across all three.** Wording can differ, claim cannot.
- **No new claims.** Everything traces to stage_20 master document.
- **No fabricated quotes / endorsements / numbers** that aren't in stage_20.

## Return summary

After writing all three files, return:

```
Wrote 3 channel adaptations:
- x_thread.md (N tweets, hook NN chars)
- linkedin_post.md (formula: F{X}, N chars)
- producthunt.md (tagline NN chars, description NN chars)

Cross-channel consistency: PASS
Master doc: /path/to/stage_20_final_review/output.md
Channel adaptations: /path/to/stage_21_deliver/
```

## Anti-patterns

- ❌ Identical text across the three channels — each format demands different muscle (thread vs single post vs PH copy)
- ❌ Over-adapting LinkedIn to look like a tweet — it's a different platform with different reading habits
- ❌ Skipping the consistency check at the end — the cross-channel novelty angle MUST match
- ❌ Padding ProductHunt with marketing prose — PH descriptions are 260 chars precisely BECAUSE filler kills them
