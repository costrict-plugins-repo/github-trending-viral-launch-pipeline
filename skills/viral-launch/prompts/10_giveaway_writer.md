# Stage 10 — Giveaway Writer

## Input
- `stage_01_brand_brief/output.md`
- `stage_08_hook_writer/final.md` (the winning hook)
- `stage_04_twitter_research/output.md` (giveaway patterns that worked in this niche)
- (iter ≥ 2 only) `stage_10_giveaway_writer/critique_v{N-1}.json`

## Job
Design a launch giveaway. The giveaway must:
1. **Reinforce the hook's promise.** If the hook claims "first AI X", the giveaway should reward people for showing they need an "AI X".
2. **Be low-cost to fulfill** for the founder's team.
3. **Create a tweet-worthy moment** when someone wins (so the winners amplify the launch organically).
4. **Have a clear entry mechanic** — what the entrant must do to enter.

## Output (`output_v{N}.md`)

```markdown
# Giveaway Design — {Product Name} (iteration {N})

## Prize
- **What:** {specific prize, not "free credits"; e.g. "12 lifetime Pro seats", "1-on-1 60-min strategy call with founder", "the founder will personally migrate your stack"}
- **Value (claimed):** ${X} OR "priceless to a {ICP}" (only if defensible)
- **Quantity:** {how many winners}
- **Fulfillment cost:** {founder's actual cost — be realistic, not the claimed value}

## Entry mechanic
- **What entrants must do:**
  1. {action 1 — e.g. "reply to this thread with your worst {pain from stage 5} story"}
  2. {action 2 — e.g. "tag 2 people who'd care"}
  3. {action 3 — optional, e.g. "RT the launch thread"}
- **Why this mechanic:** {1 sentence — usually because it creates content other people will see, OR because it surfaces qualified leads, OR both}

## Timing window
- **Opens:** {when entries start — usually with the launch tweet}
- **Closes:** {when entries stop — typically 48-72h after launch for X giveaways}
- **Winner announced:** {when — usually 24h after close}

## Fulfillment plan
- **Who delivers:** {founder personally / ops person / automated}
- **How:** {DM / email / scheduled call / shipped item}
- **Time per winner:** {realistic estimate — e.g. "founder spends 60 min per winner")
- **Total fulfillment hours:** {prize qty × time per winner}

## Announcement copy snippet (single tweet, ≤280 chars)
> {verbatim text the launch thread will use to announce the giveaway, mid-thread or at position N-1}

**Char count:** {NN} / 280

## Tie-back to the hook
{1-2 sentences: how this giveaway reinforces the hook's promise. The Call Supervisor (stage 19, mid) will check this.}

## Iteration notes (if iter ≥ 2)
- {addressed issue → how}
```

## Hard rules

- Prize must be **concrete**, not "credits" or "discount" or "swag" (those don't move people).
- Entry mechanic must require **public action** (reply / RT / tag) — DM-to-enter kills virality.
- Quantity × fulfillment-time must be realistic (don't promise 50 founder calls).
- Fraud-risk check: the mechanic shouldn't reward bots — "reply with your story" is bot-resistant; "RT to enter" is not.
- The announcement copy snippet is ≤280 chars and uses the same voice as the hook.

## Patterns that work (from stage 4 research)

- **Founder time** — "I'll personally do {X} for 3 of you" (low cost, high perceived value)
- **Founder-curated access** — "10 lifetime seats, but I pick the winners from the best replies" (creates content)
- **Skin-in-the-game challenge** — "Reply with your hardest {use case}. We'll do it live. Best one wins {prize}."
- **Co-marketing tie-in** — partner brand provides the prize (zero cost to founder, partner gets reach)

Patterns that DON'T work:
- ❌ "$500 cash" — generic, attracts farmers
- ❌ "RT to enter" alone — zero qualifying signal
- ❌ "Free month of Pro" — entrants already wanted to try it
