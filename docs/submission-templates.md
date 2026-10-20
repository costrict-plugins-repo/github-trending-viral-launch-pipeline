# Plugin directory submission templates

Two directories require submission through their UI (programmatic submission would either fail auth or violate their rules). Below are ready-to-paste texts for each. ~5 minutes of manual work.

---

## 1. Anthropic Official Plugin Marketplace

**Submit at one of (whichever you're logged into):**
- https://claude.ai/settings/plugins/submit
- https://platform.claude.com/plugins/submit

### Fields to paste

| Field | Value |
|---|---|
| **Plugin name** | `viral-launch-pipeline` |
| **Plugin repository URL** | `https://github.com/Koz-TV/viral-launch-pipeline` |
| **Marketplace name (within the repo)** | `viral-launch-pipeline` |
| **Category** | Productivity / Marketing |
| **Author** | Mike Koz (`@mixvlad`) |
| **License** | MIT |
| **Homepage** | `https://github.com/Koz-TV/viral-launch-pipeline` |

### Description (copy-paste)

> 21-agent orchestrator for viral product launches. One skill (`viral-launch`) dispatches a sequence of specialized subagents — research (YouTube/X/Reddit/Industry, parallel), hook + giveaway with rubric-based manager critique loops, body writing with a sequential specialist chain (weapons → controversy → technical → flow), a Mom Test for non-technical comprehensibility, and multi-channel delivery producing X-thread + LinkedIn post + ProductHunt copy.
>
> Built on Claude Code built-ins only (Agent, WebSearch, WebFetch, Read/Write/Edit, Bash). No external API keys. Full paper trail in `launches/<slug>/`. Resumable mid-pipeline.
>
> Manager loops force rewrites until the rubric passes (max 3 iterations per stage). Mom Test enforces mass-market readability with a 1-kickback budget. A Call Supervisor checks coherence twice per launch (after hook+giveaway, after body).

### Long description / Why this plugin

> Most AI-written launch copy fails because it's generated in one pass: hook, body, and CTA all spat out from a single prompt. This plugin enforces a multi-stage critique pipeline — every claim gets attacked, scored, and rewritten before it reaches the final draft. The architecture publicly described by viral-launch operators on X, packaged so any founder can install and run it on their own product without rebuilding the orchestration from scratch.

### Installation

```
/plugin marketplace add Koz-TV/viral-launch-pipeline
/plugin install viral-launch-pipeline@viral-launch-pipeline
```

### Test command

```
viral launch for FakeProduct
```

---

## 2. hesreallyhim/awesome-claude-code

**Submit via the GitHub Issue Form (web UI only — they ban `gh` CLI submissions):**

→ https://github.com/hesreallyhim/awesome-claude-code/issues/new?template=recommend-resource.yml

### Fields to fill in the form

| Form field | Value |
|---|---|
| **Issue title** | `[Resource]: viral-launch-pipeline` |
| **Display Name** | `viral-launch-pipeline` |
| **Category** | `Agent Skills` (they note: "currently lumping most things called 'plugins' under 'Agent Skills'") |
| **Sub-Category** | `General` |
| **Primary Link** | `https://github.com/Koz-TV/viral-launch-pipeline` |
| **Secondary Link** | (leave blank) |
| **Author Name** | `Mike Koz` |
| **Author Link** | `https://github.com/mixvlad` |
| **License** | `MIT` |

### Description field (copy-paste)

> 21-agent orchestrator plugin for viral product launches. One skill (`viral-launch`) chains 21 subagents via the `Agent` tool with manager-feedback loops, parallel web research (YouTube/X/Reddit/Industry via WebSearch + WebFetch), a sequential specialist chain on the body (weapons / controversy / technical / flow), a Mom Test, and multi-channel delivery (X-thread + LinkedIn + ProductHunt). Manager rubric gates each stage with up to 3 iterations; full paper trail in `launches/<slug>/`; resumable. No external API keys — built entirely on Claude Code built-ins.

### Additional notes for the maintainer

The plugin only makes network calls to:
- Anthropic API (via Claude Code itself)
- Public web pages via WebSearch/WebFetch (no scraping, public URLs only)

No telemetry, no auto-update, no `--dangerously-skip-permissions` requirement. Standard `/plugin install` flow.

If asked for demo evidence: run `viral launch for {any test product}` in a scratch directory — the skill writes its full intermediate artifacts to `./launches/<slug>/` so the maintainer can audit each stage's prompts and outputs.

---

## 3. Already automated (no action needed)

- **`quemsah/awesome-claude-plugins`** — automated indexer that picks up repos via GitHub topics. Our repo has the right topics (`claude-code`, `claude-plugin`, `viral-launch`, `ai-agents`, etc.) — it will appear in their next index run.

## 4. Already PR'd

- **`jqueryscript/awesome-claude-code`** — PR opened: [#288](https://github.com/jqueryscript/awesome-claude-code/pull/288). Waiting on maintainer review.
