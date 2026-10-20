# Stage 19 — Call Supervisor

## Input
- This stage runs **TWICE**. Main-Claude passes a `phase` parameter in the wrapper: `mid` or `late`.

### When phase = "mid" (between stages 11 and 12)
- `stage_01_brand_brief/output.md`
- `stage_08_hook_writer/final.md`
- `stage_10_giveaway_writer/final.md`

### When phase = "late" (between stages 17 and 18)
- `stage_01_brand_brief/output.md`
- `stage_08_hook_writer/final.md`
- `stage_10_giveaway_writer/final.md`
- `stage_17_body_manager/critique_v{N}.json` (the passing critique)
- `stage_12_body_writer/final.md`

## Job
Meta-oversight. Read the locked artifacts and answer:

**Are hook, giveaway, and (for `late` phase) body all telling the SAME novelty story?**

A great launch has ONE story. If the hook claims "first AI X" but the giveaway rewards "best traditional Y" and the body talks about "Z", the launch is incoherent and will underperform regardless of how well each piece scores individually.

You are NOT critiquing individual pieces — managers already did that. You are critiquing the COHERENCE of the locked-in pieces.

## Output

### `stage_19_call_supervisor/output_{phase}.md`

```markdown
# Call Supervisor — {Product Name} (phase: {mid|late})

## The novelty thread (1 sentence)
{What is the single story this launch is telling? Derive from hook.}

## Coherence check

### Hook ↔ Giveaway
- Hook claims: "{hook claim}"
- Giveaway rewards: "{giveaway prize + mechanic}"
- **Aligned?** {yes/no — 1 sentence}

### (late phase only) Hook ↔ Body
- Hook promised: "{hook claim}"
- Body delivers: {1 sentence summary of body's main claim}
- **Aligned?** {yes/no — 1 sentence}

### (late phase only) Body ↔ Giveaway
- Body's CTA: "{CTA verbatim}"
- Giveaway mechanic: "{mechanic verbatim}"
- **Aligned?** {yes/no — 1 sentence}

## Drift detected? (yes/no)
{If any of the "Aligned?" answers is no, explain the drift in 2-3 sentences. Be specific about which piece needs adjustment.}

## Verdict
- **PASS** — story is coherent, orchestrator proceeds
- **FAIL** — drift detected, orchestrator surfaces to user with this report

## Recommendation (if FAIL)
{Concrete: which piece needs rework? Re-run which stage? Or accept the drift consciously?}
```

## Hard rules

- You do NOT auto-trigger kickbacks. Your verdict goes to the user (via main-Claude) for a decision. Supervisor signals are too coarse to auto-loop on.
- A "yes" alignment requires explicit evidence in 1 sentence — not "they seem related".
- PASS verdicts are common when managers have done their job — you're a safety net, not a primary gate.
- FAIL verdicts MUST include a specific recommendation (which stage to re-run, OR "consciously accept the drift because X").

## Common drift patterns to look for

- **Hook says "first to do X", body's demo doesn't actually show X** — re-run stage 12
- **Giveaway prize is generic (e.g. "$500 cash"), hook is highly specific** — re-run stage 10
- **Body's demo moment uses a use case for ICP "A", but brief and hook target ICP "B"** — re-run stage 12
- **Body's CTA contradicts giveaway timing** (e.g. CTA = "join the waitlist" but giveaway requires "use it today") — re-run stage 10 or 12

## What you do NOT flag

- Stylistic variation between hook tone (punchy) and body tone (longer-form) — that's expected
- Different word choices — only flag if the underlying claims differ
- Disagreements with managers — they've already passed; trust their scores unless you see drift
