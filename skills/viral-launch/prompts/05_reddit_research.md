# Stage 05 — Reddit Research

## Input
- `stage_02_keywords/output.md`
- `references/search-query-templates.md` (section: Stage 5)

## Job
Mine Reddit for **verbatim customer pain quotes** that the launch can echo back. These quotes will be lifted (with attribution) into hooks and body copy in later stages.

## Tools
- `WebSearch` — run the 4 primary queries plus 2-3 subreddit-targeted queries
- `WebFetch` — fetch top thread URLs if WebSearch snippets are too short for full quotes
- No fabrication. Made-up Reddit quotes are an instant launch killer.

## Output (`output.md`)

```markdown
# Reddit Research — {Product Name}

## Queries run
- {query 1} → {N results}
- ...

## Verbatim pain quotes (5-10)

Format: `> "<exact quote ≤120 chars>"` followed by attribution.

1. > "{exact quote — keep typos and casing}"
   — r/{subreddit}, [{date or "N years ago"}]({permalink})

2. > "..."
   — r/{subreddit}, [link]({permalink})

...

## User vocabulary (words from above quotes that are NOT in the brand brief)
- "{word/phrase}" — how it's used
- "{word/phrase}" — ...
- ...

## Active subreddits (where this pain shows up most)
- **r/{sub}** — {member count if visible} — typical thread vibe
- **r/{sub}** — ...

## Sentiment summary (1 paragraph)
{What's the emotional shape of the pain? Resignation, anger, sarcastic exhaustion, hope-with-skepticism? Stage 8 Hook Writer needs the emotional charge to mirror.}

## Sources
- {permalink 1}
- ...
```

## Hard rules

- **Every quote MUST have a permalink.** If WebSearch only gives you a snippet, `WebFetch` the thread URL to recover the quote in context.
- Quotes ≤120 chars. Trim with `[…]` if needed; don't paraphrase.
- Keep typos, lowercase, casual punctuation — that's the voice. Fixing it loses the signal.
- 5-10 quotes, no more. The Hook Writer doesn't have time to read 30.
- If the category is too quiet on Reddit (e.g. very niche B2B), write `## DEAD ENDS` listing the queries that produced nothing. Don't pad.

## Anti-patterns

- ❌ Quoting comments by what they "say" — must be verbatim
- ❌ Polishing typos — lose signal
- ❌ Mixing in user reviews from G2/Capterra — those are different signal (use stage 6 for that)
