# Manager critique rubric (JSON schema)

All **manager** stages (9 hook_manager, 11 giveaway_manager, 17 body_manager) write exactly this JSON to `critique_v{N}.json`:

```json
{
  "pass": false,
  "score": 6,
  "iteration": 1,
  "winner_id": null,
  "rubric_used": "hook_three_question_test",
  "scores_by_axis": {
    "clarity": 7,
    "novelty": 5,
    "urgency": 6,
    "voice": 8,
    "three_question_pass": false
  },
  "issues": [
    {
      "severity": "blocking",
      "where": "candidate #3, sentence 1",
      "what": "Generic opener — 'Introducing X' has no urgency hook.",
      "fix": "Open with the verbatim Reddit pain quote from stage_05 line 42, then introduce the product."
    },
    {
      "severity": "major",
      "where": "candidate #1, line 2",
      "what": "Claim that 'never existed before' is undefensible — competitor Foo shipped this in 2024.",
      "fix": "Either drop the claim or narrow it ('first to do X with Y constraint')."
    },
    {
      "severity": "minor",
      "where": "candidate #5",
      "what": "Hashtag in hook reduces reach on X.",
      "fix": "Move hashtags to the final reply tweet."
    }
  ],
  "rewrite_guidance": "Pivot every candidate to lead with concrete pain. Drop unverifiable 'first/only' claims. Aim for a hook that answers all three test questions in the first 280 chars."
}
```

## Pass rule (computed by manager, but the orchestrator also re-checks)

```
pass = (score >= 8)
   AND (no issues with severity == "blocking")
   AND (winner_id != null)
```

A pass=true critique **must** also write `final.md` in the sibling worker's directory containing only the winning artifact (no commentary, no scores).

## Score axes per stage

### Stage 9 hook_manager — uses `references/hook-three-question-test.md`
- `clarity` — would a stranger know what's being launched from this line alone?
- `novelty` — does it claim something specifically new, or generic marketing language?
- `urgency` — does it answer "why now"?
- `voice` — does it match the founder's voice from stage_01 brief?
- `three_question_pass` — boolean: passes the what/why-now/why-never-existed test?

Pass requires `three_question_pass = true` PLUS overall score ≥ 8.

### Stage 11 giveaway_manager
- `on_brand` — does the giveaway prize reinforce the hook's promise?
- `low_fraud_risk` — entry mechanic doesn't reward botting/farming
- `fulfillable` — small team can deliver promised prize without operational pain
- `virality_multiplier` — does the entry mechanic require the entrant to share/post?
- `tweet_worthy_moment` — does winning the giveaway create a moment worth tweeting?

### Stage 17 body_manager
- `hook_landed` — first paragraph delivers what the chosen hook promised
- `demo_concrete` — has at least one specific, named, scene-like demo moment (not "imagine if…")
- `novelty_defended` — every novelty claim has either evidence or a credible mechanism
- `giveaway_natural` — giveaway appears in a beat that earns it, not bolted-on
- `cta_clear` — single CTA, unambiguous, low friction
- `no_ai_slop` — none of these phrases: "in today's fast-paced", "game-changer", "revolutionize", "leverage", "seamlessly", "robust", "powerful", "intelligent" (unless directly quoting)
- `no_brief_contradictions` — every fact in the body is traceable to stage_01 brief or earlier research

## Severity definitions

- **blocking** — pass is mathematically impossible while this exists. Examples: claim is factually false per brief; hook has no novelty axis at all; body contradicts stage_01.
- **major** — would torpedo a launch in the wild. Pass=false unless fixed.
- **minor** — improvement, not a blocker. Pass can be true with minor issues unresolved.

## What the next iteration's worker receives

When the orchestrator dispatches iteration N>1 of the worker, the wrapper's `DEPENDENCY_FILE_LIST` includes the previous `critique_v{N-1}.json`. The wrapper additionally injects this instruction line:

> "Address every BLOCKING and MAJOR issue in critique_v{N-1}.json. Do not regress on what already passed. If you disagree with a critique item, leave a 1-line `<!-- DISAGREE: <reason> -->` HTML comment next to the relevant candidate and address the next-most-actionable issue."

## What happens after iteration 3 fails

Orchestrator surfaces to user:
- The 3 worker outputs (`output_v1.md`, `output_v2.md`, `output_v3.md`)
- The 3 critiques
- The highest-scoring candidate (by `score` field, then by `winner_id` if tied)
- Three-option prompt: **accept best-of-3** / **manual edit** / **abort launch**
