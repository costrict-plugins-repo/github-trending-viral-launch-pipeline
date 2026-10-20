# Stage 20 — Final Review

## Input
- `stage_08_hook_writer/final.md`
- `stage_10_giveaway_writer/final.md`
- `stage_12_body_writer/final.md`
- `stage_19_call_supervisor/output_mid.md`
- `stage_19_call_supervisor/output_late.md`
- `stage_01_brand_brief/output.md` (for cross-checking names, capitalization, facts)

## Job
**One final polish pass.** Assemble the locked hook + body + giveaway into a single **master launch document** that stage 21 (Deliver) will adapt to channels.

You may make small fixes:
- Typos
- Capitalization (product names, company names, founder names — match stage_01 brief exactly)
- Punctuation consistency
- A single sentence where a transition was rough between locked pieces

You may NOT:
- Change the hook
- Change the body's structure or claims
- Change the giveaway prize or mechanic
- Re-introduce filler

## Output (`stage_20_final_review/output.md`)

```markdown
# Master Launch Document — {Product Name}

## Hook (≤280 chars, X tweet 1 ready)
{verbatim hook from stage_08/final}

**Char count:** {N} / 280

---

## Body (channel-agnostic, 600-1000 words)

{full body from stage_12/final, with any micro-fixes applied}

**Word count:** {N}

---

## Giveaway announcement snippet (X-thread tweet N-1, ≤280 chars)
{verbatim from stage_10/final}

**Char count:** {N} / 280

---

## Giveaway full details (for ProductHunt + LinkedIn first comment)
- **Prize:** {from stage_10/final}
- **Entry:** {mechanic}
- **Timing:** {window}
- **Winners announced:** {when}
- **Fulfillment:** {how}

---

## CTA (single, low-friction)
{verbatim from body's last paragraph}

---

## Cross-channel facts (one source of truth)
- **Product name (exact capitalization):** {Product Name}
- **Company:** {Company}
- **Founder(s):** {Names with @ handles}
- **Product URL:** {URL}
- **Launch date:** {YYYY-MM-DD}

## Micro-fixes applied
- {fix 1, where, why}
- {fix 2 ...}
- (or "no fixes needed — locked pieces assemble cleanly")

## Supervisor verdicts (for paper trail)
- Mid-phase: {PASS/FAIL with 1-line note}
- Late-phase: {PASS/FAIL with 1-line note}

## Ready for stage 21 (Deliver)
☑ Hook ≤280 chars
☑ Body 600-1000 words
☑ Giveaway snippet ≤280 chars
☑ CTA present
☑ All product/founder names match brief exactly
☑ No em dashes, no disqualifier words, no AI-slop tells
☑ All supervisor verdicts PASS (or user explicitly accepted FAIL drift)
```

## Hard rules

- **You do not rewrite.** Polish only.
- All checkmarks at the bottom must be true (☑) before you return. If any is false, log the issue in `## Micro-fixes applied` with status "UNRESOLVED — escalate to user" and main-Claude will halt.
- The hook, body, and giveaway are LOCKED. Touch only typos, capitalization, and a single transition sentence if needed.
- "Cross-channel facts" must be derived directly from stage_01 brief. If brief disagrees with what's currently in the body (e.g. founder's @ handle), update the body and log in micro-fixes.

## Anti-patterns

- ❌ Rewriting the hook because you think you can improve it — outside scope
- ❌ Restructuring the body — outside scope
- ❌ "Improving" the CTA — outside scope
- ❌ Adding new claims — outside scope
- ❌ Forgetting to update product name capitalization if the body has it wrong
