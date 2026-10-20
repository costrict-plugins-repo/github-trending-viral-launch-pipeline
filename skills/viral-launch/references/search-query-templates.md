# Search query templates for research agents

Stages 3-6 (YouTube, X/Twitter, Reddit, Industry) use `WebSearch` and `WebFetch`. Each agent must run **at least 4 distinct search queries**, then `WebFetch` the top 2-3 URLs from the highest-signal results.

## Stage 3: YouTube Research

Inputs: `category_terms`, `competitor_terms`, `outcome_terms` from stage 2.

**Required queries (run all four):**
```
site:youtube.com "{primary_category_term}" review
site:youtube.com "{primary_outcome_term}" tutorial OR demo
site:youtube.com "{top_competitor}" review OR alternative
"{primary_category_term}" "million views" OR "viral" site:youtube.com
```

**Extraction targets per result:**
- Video title (the hook the channel used)
- Channel name + subscriber tier (small=<10k, mid=10k-500k, big=>500k)
- View count if visible (or "≥X" from typical channel range)
- Why this title beat the channel's median (hook pattern: number / claim / question / contrast)
- Comments sentiment if `WebFetch` of the URL exposes top comment (often does)

**Outlier rule:** an "outlier" = title that's 5×+ above channel median, OR a video on a small channel with disproportionate views (e.g. 100k views on a 5k-sub channel). Flag those explicitly.

**Output:** markdown table (8-12 rows) + 1-paragraph "novelty patterns" synthesis listing the 2-3 hook patterns that repeat across outliers.

## Stage 4: X/Twitter Research

Inputs: same keyword lists.

**Required queries:**
```
site:x.com "{primary_category_term}" launch OR launching
site:x.com "{primary_outcome_term}" thread
site:twitter.com "{top_competitor}" launch
"{primary_category_term}" "1/" OR "🧵" site:x.com
```

**Fallback if X search is rate-limited or sparse:**
```
site:nitter.net "{primary_category_term}" launch
site:nitter.net "{top_competitor}"
```

**Extraction targets per viral launch tweet/thread:**
- The hook (tweet 1 text, ≤280 chars)
- Thread length (number of follow-up tweets) — extract by `WebFetch`-ing the URL and counting
- Engagement signals (likes / retweets / replies; estimate if not shown)
- Format: thread / single tweet with video / single tweet with image / quote-RT chain
- Did they use a giveaway? what kind?

**Output:** markdown table (8-12 rows) + synthesis paragraph: "what launch posts go viral in this category on X."

## Stage 5: Reddit Research

Inputs: `pain_terms`, `category_terms`.

**Required queries:**
```
site:reddit.com "{primary_pain_term}"
site:reddit.com "I hate" OR "frustrated with" "{primary_category_term}"
site:reddit.com "alternative to" "{top_competitor}"
site:reddit.com "is there a way to" "{primary_outcome_term}"
```

**Subreddit-targeted (run 2-3 of these too, after eyeballing top subs from above):**
```
site:reddit.com/r/{likely_sub_1} "{primary_pain_term}"
site:reddit.com/r/{likely_sub_2} "{primary_outcome_term}"
```

**Extraction targets:**
- **Verbatim pain quotes** (5-10 of them) — must be exact quote, ≤120 chars, with permalink
- **Vocabulary the user actually uses** — words from the quotes that are NOT in the brand brief
- Subreddits where the pain shows up (so giveaway and CTA can target them later)

**Output:** `## Verbatim pain quotes` section + `## User vocabulary` section + `## Active subreddits` list.

## Stage 6: Industry Research

Inputs: `competitor_terms`, `category_terms`.

**Required queries:**
```
"{top_competitor_1}" pricing
"{top_competitor_2}" alternative OR competitor
"{primary_category_term}" market size 2025 OR 2026
best "{primary_category_term}" tools
```

**Then `WebFetch` for top 3-5 competitor homepages.** Extract from each:
- Headline (the H1)
- Sub-headline / supporting line
- First CTA button text
- Pricing tier names + entry-tier $/mo if visible

**Output:** markdown competitor matrix (cols: name, headline, sub-headline, CTA, entry price) + 1-paragraph "positioning gap": which axis is empty? what is every competitor saying that the user could counter-position against?

## Hard rules for all research agents

1. **No fabrication.** If a search returns nothing, write `No results.` Don't invent fake quotes, view counts, or competitor data.
2. **Cite every claim with the source URL.** Inline like `(source: https://reddit.com/r/.../comment/abc123)`.
3. **WebSearch first, WebFetch only on the top 2-3 URLs.** Each WebFetch is expensive — pick the highest-signal ones.
4. **Stop at 12 results per agent.** More breadth doesn't beat the first 8-12 outliers.
5. **Flag dead ends to the orchestrator.** If a category has no viral launches on X, say so explicitly in the synthesis paragraph — the Hook Writer needs to know it's working in a virgin niche.
