# Stage 01 — Brand Brief

## Input
- `input.md` in your stage directory contains whatever raw facts the user provided to main-Claude (product URL, founder name, rough pitch, etc.).
- You have no other dependencies.

## Job
Produce a **structured brand brief** that all downstream stages will rely on as the source of truth.

If `input.md` is missing key facts, your output should include a `## OPEN QUESTIONS` section at the end listing them. The orchestrator will then prompt the user, append answers to `input.md`, and re-dispatch you.

Do NOT fabricate facts. If you don't know the launch date, write "unknown — ask user". If the founder voice samples are absent, say so.

## Required output sections (`output.md`)

Use these exact headings, in this order:

```markdown
# Brand Brief — {Product Name}

## Product
- **Name:** {capitalized exactly as the founder writes it}
- **One-line positioning:** {≤120 chars, what + for whom}
- **Category:** {how a stranger would describe the bucket}
- **URL:** {product URL or "not yet live"}

## Company / Founder
- **Company:** {name}
- **Founder(s):** {names, with Twitter/X handles if available}
- **Founder voice samples:** {2-3 quotes from past posts, OR "no public samples — derive voice from input.md"}

## ICP (Ideal Customer Profile)
- **Who:** {role + company stage + team size}
- **Where they hang out:** {subreddits, communities, hashtags — guesses ok, mark as such}
- **What they're using today:** {current solutions, even if homemade}

## Pricing & business model
- {tier name → entry price/mo, key feature gate}
- {next tier ...}
- {or "free tier only" / "enterprise contact" / "unknown — ask"}

## What's genuinely new (the novelty thesis)
- {1-3 bullet points. For each: claim + WHY it's defensible (a feature, a mechanism, a constraint competitors have)}

## Top-3 features (concrete, demoable)
1. **{Feature name}** — {what it does in 1 sentence} — {proof: screenshot URL, demo video, or "live in app"}
2. ...
3. ...

## Top-3 known objections
1. {objection a smart skeptic would raise} — {how the product answers it}
2. ...
3. ...

## Launch details
- **Launch date:** {YYYY-MM-DD or "unknown — ask"}
- **Channels:** X-thread + LinkedIn + ProductHunt (default — flag if any are off-table)
- **Existing audience:** {founder follower counts on each channel; "small/medium/large" ok}
- **Distribution help:** {investors, partners, paid amplification — "none" is fine}

## OPEN QUESTIONS
- {only present if facts are missing; the orchestrator will surface these to the user}
```

## Quality bar (self-check before writing)

- Every line above is grounded in `input.md` or marked as a known unknown
- Founder voice samples (if any) are verbatim quotes, not paraphrased
- "What's genuinely new" lists 1-3 items, NOT 5+ (more = weaker)
- No marketing language in your own writing: just facts

## Anti-patterns

- ❌ "powerful, intuitive interface" — opinion, not fact
- ❌ "first AI to do X" without checking — fail the defensibility test downstream
- ❌ Filling OPEN QUESTIONS with trivia ("favorite color of founder?") — only ask what stages 2-21 actually need
