# The three-question hook test

Every hook the pipeline produces must pass this test. It's the single non-negotiable rule for stages 8 (Hook Writer) and 9 (Hook Manager).

## The three questions

A hook is a pass **only** if, after reading it once with no other context, a stranger can answer all three:

1. **What is this?** — what product, what category, what does it actually do
2. **Why does it matter NOW?** — what changed, what timing makes this urgent, why should the reader care this minute
3. **Why has nothing like this existed before?** — the novelty axis, the specific reason this is the first of its kind

If the reader can only answer 2 of 3 → fail. If they answer all 3 → pass on this axis (score still depends on clarity, urgency, voice).

## Worked examples

### PASS: "Introducing the world's first AI ad orchestrator that uses your content to kill AI slop."

- **What?** AI ad orchestrator
- **Why now?** "AI slop" — the recent flood of generic AI-made ads is recognizable to anyone in marketing
- **Why never existed?** Uses YOUR content (most AI tools generate from prompts, this orchestrates around assets)

### FAIL: "Introducing the world's first AI ad maker."

- **What?** AI ad maker. ✓
- **Why now?** Nothing. AI ad makers are 2-year-old commodity.
- **Why never existed?** Nothing — there are dozens already.

Article-quoted as the canonical bad example.

### PASS: "We just shipped the first IDE that understands your whole codebase, not just your open tab."

- **What?** IDE
- **Why now?** Implicit — codebase scale exceeded existing tools
- **Why never existed?** Context window constraint; only model X just made this feasible (defensible if true)

### FAIL: "Excited to announce we raised $20M to help teams improve their marketing workflows."

- **What?** Funding announcement. ✓ (but funding ≠ product)
- **Why now?** Nothing — the reader doesn't care about the round
- **Why never existed?** Nothing — "marketing workflows" is a dead phrase

Article-quoted as the canonical bad example.

### PASS: "We turned a 5-hour customer support shift into a 12-minute review queue. Live demo in 30 sec."

- **What?** Implicit: customer support automation
- **Why now?** Concrete time delta — 5h → 12m
- **Why never existed?** Implicit — review queue framing (you stay in the loop) is the novelty axis vs. full autopilot competitors

### FAIL: "We built a powerful, seamless platform for modern teams."

- **What?** "Platform" — useless, says nothing
- **Why now?** Nothing
- **Why never existed?** Nothing — "powerful", "seamless", "modern" are dead generic

## Disqualifiers (auto-fail before scoring)

A hook **auto-fails** before any scoring if it contains:

- The word "platform" without a specific noun next to it (e.g. "platform for X" ok; "the platform" fail)
- Words: "powerful", "seamless", "robust", "intelligent", "modern", "enterprise-grade", "AI-powered" (alone), "next-generation", "revolutionary", "game-changing", "leverage", "ecosystem"
- "Excited to announce" / "Thrilled to share" / "Today we're proud to" — any feel-good opener that puts the speaker before the claim
- A funding amount as the lead claim (raise + product can coexist, but raise as the hook is fail)
- Em dashes (`—`) — purely an AI-tell

## Defensibility check (stage 9 manager applies)

A hook can pass the 3-question test and still fail the rubric if the "why never existed" claim is **undefensible** by the brief or research.

Examples:
- "First AI to write working code" — false, fail
- "First AI to write a working iOS app from a single prompt" — defensible if true, check brief
- "First IDE with full codebase awareness" — false (Cursor, Codeium claim similar), fail
- "First IDE that builds a typed call graph from 200k tokens" — defensible IF the brief substantiates it

The Hook Manager must trace every novelty claim back to either:
1. A specific feature in stage_01 brief, OR
2. A research finding showing competitors lack this (stage 6 industry research), OR
3. A mechanism the founder can explain in one sentence

If none of the three back it up → the issue is `severity: "blocking"` regardless of how good the writing is.

## Hook length budget by channel

- **X tweet 1**: ≤280 chars. Hook Writer outputs in this budget.
- **LinkedIn**: ≤210 chars to land before "see more" truncation. Stage 21 may compress the chosen hook.
- **ProductHunt tagline**: ≤60 chars. Stage 21 derives a compressed version — the Hook Writer doesn't aim for this.

Hook Writer produces 5 candidates ALL within ≤280 chars. Stage 21 handles channel re-fits.
