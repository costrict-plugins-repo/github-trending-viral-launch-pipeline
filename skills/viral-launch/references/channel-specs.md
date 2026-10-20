# Multi-channel delivery specs (stage 21)

Stage 21 (`deliver`) reads `stage_20_final_review/output.md` (the master launch document) and adapts it into **three channel-specific artifacts**. All three must ship the same novelty angle, but with each channel's own format constraints.

## Channel 1: X-thread (`x_thread.md`)

### Hard constraints

- **Tweet 1 = HOOK**: ≤280 characters INCLUDING any link or media reference. No exceptions.
- **Thread length**: 7-12 tweets total (hook + 6-11 body tweets). 5 or fewer reads as thin. 13+ loses retention.
- **Each tweet ≤280 chars.** Use `len()` on each line before writing.
- **One idea per tweet.** If a tweet has two ideas, split.
- **No threading hashtags in tweet 1.** Hashtags reduce reach on X. Move to last tweet if at all.
- **Giveaway tweet sits at position N-1**, not N. Final tweet is always CTA + link.

### Output template

```markdown
# X-thread for {product_name}

## Tweet 1 (HOOK)
[≤280 char hook, must answer what / why-now / why-never-existed]

## Tweets 2-{N-2} (BODY)
2/ [demo moment opener — one specific scene]
3/ [the problem framing — verbatim pain from reddit if available]
4/ ["what changed under the hood" — concrete mechanism]
5/ [before/after metric — exact numbers from brief]
6/ [contrarian beat from stage 14]
7/ [credibility detail from stage 15]
...

## Tweet N-1 (GIVEAWAY)
{N-1}/ [giveaway announcement from stage 10 final]

## Tweet N (CTA + LINK)
{N}/ Try it: {product_url}
     [one-line ask: try / retweet / reply with X]

## Image/video notes
- Tweet 1: attach [demo video / before-after screenshot / hero image]
- Tweet 5: attach [the metric chart / receipt screenshot]

## Posting metadata
- Best window: Tue/Wed/Thu 9-11am ET (founder timezone if different — note here)
- Format: thread (chained replies, not separate tweets)
- Pin tweet 1 to profile: yes
- Boost: optionally amplify via founder's main account if launching from product handle
```

## Channel 2: LinkedIn post (`linkedin_post.md`)

### Hard constraints

- **Single post** (no carousel, no document). LinkedIn rewards single-format right now.
- **900-1,300 characters total.** Under 900 reads thin, over 1,300 loses second-half retention.
- **Hook in first 210 characters**, before the `… see more` truncation point.
- **Double line breaks between ideas.** Single line breaks render as wrapped paragraphs on LinkedIn.
- **0-2 hashtags max**, placed at the end.
- **No em dashes** anywhere (em-dash heuristic flags AI on LinkedIn).
- **No external links in the post body.** Links go in the first comment.

### Formula selection

If `~/.claude/plugins/marketplaces/linkedin-skills/skills/linkedin-post-writer/references/hook-formulas.md` exists (check via `Bash test -f`), pick one of:

| When to use | Formula |
|---|---|
| Launch reframes a whole category | **F1 Platform Risk Anaphora** ("LinkedIn could die / Twitter could die / But X doesn't...") |
| Launch closes a chapter on something | **F2 R.I.P. Obituary** |
| Launch is a build-log with hard numbers | **F7 Odd-Precision Money Ledger** ("I spent $4,372 on this. Here's what I learned.") |
| Launch announces something contrarian | **F10 Contrarian + Historical Receipts** |

If the linkedin-skills plugin is not installed, fall back to **generic-launch-narrative**: hook (3 lines) → problem (2 lines) → product moment (3 lines) → metric (1 line) → CTA (1 line, "comment X to get the link").

### Output template

```markdown
# LinkedIn post for {product_name}

## Formula used
{F1 / F7 / F10 / generic-launch-narrative}

## Char count
{exact count}

## Hook (first 210 chars — must land before "see more")
{verbatim first 210 chars}

## Full post
{900-1,300 chars, double line breaks, 0-2 hashtags at end, no em dashes}

## First comment (link goes here, NEVER in post body)
{1-2 line CTA + product URL}

## Posting metadata
- Best window: Tue/Wed/Thu 7:30-9:00am local
- Post type: text-only (highest reach in 2026) OR text + 1 image (close second)
- Hashtags: {0-2 from end of post}
```

## Channel 3: ProductHunt (`producthunt.md`)

### Hard constraints

- **Tagline**: ≤60 chars. (PH cuts off at this; longer becomes ellipsis.)
- **Description**: ≤260 chars. Used as the social preview.
- **Maker's first comment**: 3 paragraphs, no fluff.
- **3 topics maximum.** Pick from PH's existing taxonomy (e.g. "Artificial Intelligence", "Productivity", "Developer Tools").
- **Gallery**: hero image first, demo video second (if available), feature shots after.

### Output template

```markdown
# ProductHunt launch for {product_name}

## Tagline (≤60 chars)
{tagline}
[char count: NN / 60]

## Description (≤260 chars)
{description}
[char count: NNN / 260]

## Maker's first comment

### Paragraph 1 — Why we built it
{2-3 sentences, founder voice, specific origin moment from stage_01 brief}

### Paragraph 2 — What's new (the hook claim)
{2-3 sentences, the chosen hook re-cast for a hunter audience — slightly more technical, less Twitter-flavored}

### Paragraph 3 — How to engage today
{Specific ask: try the live demo, comment with use case X, drop a question, etc. Mention the giveaway if running.}

## Topics (3, from PH's taxonomy)
- {Topic 1}
- {Topic 2}
- {Topic 3}

## Gallery suggestions (priority order)
1. Hero image — {description of what to show}
2. Demo video (30-60s) — {scene-by-scene}
3. {Feature shot 1}
4. {Feature shot 2}

## Posting metadata
- Launch day: {weekday — Tue/Wed/Thu best; avoid Mon/Fri}
- Launch time: 12:01 AM Pacific (PH's day rollover)
- Hunter: {self-hunt OR named hunter from network}
- Pre-launch coming-soon page: {URL or "create one"}
```

## Cross-channel consistency check (last step of stage 21)

Before declaring `deliver` complete, the agent must verify:

1. **Same novelty angle in all three.** The hook claim from X tweet 1 should be detectable in LinkedIn post hook AND in ProductHunt tagline. Not identical wording — same claim.
2. **Same product name capitalization** everywhere.
3. **Same metric numbers** if any number from the brief shows up in 2+ channels, the digits must match.
4. **Giveaway is mentioned in X (yes), LinkedIn (optional), ProductHunt (yes if running)** — not contradictory.

If any check fails, the agent rewrites and re-verifies before returning.
