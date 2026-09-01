# How AI Credits Are Charged — Copilot Pro, Pro+, and Max+ Individual Plan Scenarios

> **Date:** September 1, 2026
> **Plans:** Copilot Pro — $10/month · Copilot Pro+ — $39/month · Copilot Max+ — $79/month *(projected)*
> **Effective:** June 1, 2026
> **Source:** [GitHub Docs — Usage-Based Billing](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises), [Models & Pricing](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)

> ⚠️ **A note on Copilot Max+:** As of this writing, GitHub has publicly announced **Free, Pro, and Pro+** as the individual plan tiers. **Max+ is not an officially announced GitHub plan.** It is modeled here as a **projected/hypothetical top tier**, extrapolated from the existing Free → Pro → Pro+ pricing pattern (each tier roughly 2–4x the previous in price and included credits), for planning and "what-if" purposes only. Treat all Max+ figures as **estimates**, not commitments — update this document once/if GitHub announces an official spec.

---

## Table of Contents

1. [Plan Overview](#1-plan-overview)
2. [How Individual Plan Billing Works](#2-how-individual-plan-billing-works)
3. [Token Estimation Quick Reference](#3-token-estimation-quick-reference)
4. [Copilot Pro Scenarios](#4-copilot-pro-scenarios)
5. [Copilot Pro+ Scenarios](#5-copilot-pro-scenarios-1)
6. [Copilot Max+ Scenarios (Projected)](#6-copilot-max-scenarios-projected)
7. [Cross-Plan Comparison](#7-cross-plan-comparison)
8. [Which Plan Fits Which Developer Profile](#8-which-plan-fits-which-developer-profile)
9. [What Happens When Credits Run Out](#9-what-happens-when-credits-run-out)

---

## 1. Plan Overview

| Plan | Monthly Price | Included AI Credits | Included $ Value | Status |
|------|---------------|---------------------|-------------------|--------|
| **Copilot Free** | $0 | Limited (50 premium requests/month under current model) | $0 | Official |
| **Copilot Pro** | $10/month | **1,000 credits** | $10 | Official |
| **Copilot Pro+** | $39/month | **3,900 credits** | $39 | Official |
| **Copilot Max+** *(projected)* | $79/month | **7,900 credits** *(est.)* | $79 *(est.)* | **Not yet announced** |

Unlike Business/Enterprise plans, individual plan credits are **not pooled** — each account has its own personal allowance that does not share with other users.

---

## 2. How Individual Plan Billing Works

### The Basics

| Item | Copilot Pro | Copilot Pro+ | Copilot Max+ *(projected)* |
|------|-------------|--------------|------------------------------|
| Seat price | **$10/month** | **$39/month** | **$79/month** *(est.)* |
| Included AI Credits | **1,000 credits** | **3,900 credits** | **7,900 credits** *(est.)* |
| Pooling | None — personal allowance only | None — personal allowance only | None — personal allowance only |
| Credit value | 1 AI Credit = $0.01 USD | 1 AI Credit = $0.01 USD | 1 AI Credit = $0.01 USD |
| Code completions | **Free** — not billed in AI Credits | **Free** | **Free** |
| Next Edit Suggestions | **Free** — not billed in AI Credits | **Free** | **Free** |
| Overage / additional usage | Not available on consumer plans today | Not available on consumer plans today | Assumed not available *(est.)* |

### The Formula

```
┌──────────────────────┐   ┌─────────────────────────────┐   ┌────────────────────────────┐
│ Input tokens         │ + │ Cache read tokens           │ + │ Output tokens              │
│   × input rate       │   │   × cache rate              │   │   × output rate            │
└──────────────────────┘   └─────────────────────────────┘   └────────────────────────────┘
                                        ↓
                            = dollar cost → AI Credits
```

**Example:**
> 500,000 input tokens at $5 per million = **$2.50** = **250 AI Credits**

### Key Rules

- **Rates vary by model and token type** — each model has its own input, cache, and output rate
- **Tokens are not interchangeable** — input, cache read, and output tokens are priced separately
- **Code completions and Next Edit Suggestions remain included** on all three tiers — they do not consume AI Credits
- **No pooling** — unlike Business/Enterprise, unused individual credits do not roll over or share with anyone
- **Unused allowance does not roll over** month to month

---

## 3. Token Estimation Quick Reference

| Item | Approximate Tokens |
|------|--------------------|
| 1 word | ~1.3 tokens |
| 1 line of code | ~15–25 tokens |
| 1 typical source file (200 lines) | ~3,000–5,000 tokens |
| A short chat prompt | ~200–500 tokens |
| A detailed prompt with context | ~2,000–8,000 tokens |
| A short response (explanation) | ~300–800 tokens |
| A full function generation | ~500–2,000 tokens |
| A multi-file agent output | ~10,000–50,000 tokens |

---

## 4. Copilot Pro Scenarios

> **Plan:** $10/month → **1,000 AI Credits included**

### Scenario P1: Light User — Chat Only

> Hobbyist/student who asks quick questions, no agent mode

| # | Activity | Model | Credits |
|---|----------|-------|---------|
| 1–15 | 15 quick chats per day | GPT-5 mini | 1.5 |
| 16–18 | 3 short code explanations | GPT-4.1 | 1.2 |
| | **Daily Total** | | **2.7 credits** |

| | |
|--|--|
| **Monthly (22 working days)** | **~59 credits** |
| **% of Pro plan used** | **5.9%** |
| **Headroom** | **~941 credits remaining** |

> ✅ Comfortably fits — this is the classic "learning to code" or occasional-use profile.

---

### Scenario P2: Moderate User — Daily Chat + Occasional Code Gen

> Freelancer who chats daily and generates small functions, no agent sessions

| # | Activity | Model | Credits |
|---|----------|-------|---------|
| 1–10 | 10 quick chats | GPT-5 mini | 1.0 |
| 11–14 | 4 function/utility generations | GPT-4.1 | 2.2 |
| 15 | 1 unit test generation | GPT-5.2 | 5.8 |
| 16 | 1 code explanation (~150 lines) | Claude Sonnet 4 | 3.2 |
| | **Daily Total** | | **12.2 credits** |

| | |
|--|--|
| **Monthly (22 working days)** | **~268 credits** |
| **% of Pro plan used** | **26.8%** |
| **Headroom** | **~732 credits remaining** |

> ✅ Fits well within Pro, with plenty of margin for occasional heavier days.

---

### Scenario P3: Active Developer — Mixed Workflow with Light Agent Use

> Same "active developer" day used in the Business plan scenarios, applied to an individual Pro subscriber

| # | Activity | Model | Credits |
|---|----------|-------|---------|
| 1–5 | 5 quick chats | GPT-5 mini | 0.5 |
| 6 | 1 small agent session (2 files) | Claude Sonnet 4 | 25.0 |
| 7 | Quick chat about API design | GPT-5 mini | 0.2 |
| 8 | Generate PR description | GPT-5 mini | 0.3 |
| 9 | Code review (auto-selected) | Auto-selected | 4.9 |
| 10 | Explain a colleague's complex code | Claude Sonnet 4 | 3.2 |
| 11 | Generate documentation | GPT-5 mini | 1.0 |
| | **Daily Total** | | **35.1 credits** |

| | |
|--|--|
| **Monthly (22 working days)** | **~772 credits** |
| **Pro plan included** | 1,000 credits |
| **Headroom** | **~228 credits (23%)** |

> ⚠️ This developer is close to the edge. One extra agent session (~25–30 credits) most days would push them over 1,000 — **Pro+ is the safer choice** if agent use becomes regular.

---

### Scenario P4: Over the Limit — Daily Agent User on Pro

> Developer who runs 2 agent sessions/day — a workload Pro was not designed for

| # | Activity | Model | Credits |
|---|----------|-------|---------|
| 1–5 | 5 quick chats | GPT-5 mini | 0.5 |
| 6–7 | 2 agent sessions (~3 files each) | Claude Sonnet 4 | 50.0 |
| 8 | 1 code review | Auto-selected | 5.0 |
| | **Daily Total** | | **55.5 credits** |

| | |
|--|--|
| **Monthly (22 working days)** | **~1,221 credits** |
| **Pro plan included** | 1,000 credits |
| **Shortfall** | **~221 credits over allowance** |

> ❌ This usage pattern exceeds Copilot Pro roughly **10 working days before month end**, assuming no overage is available on individual plans. **Upgrade to Pro+ is required** to sustain this pattern all month.

---

## 5. Copilot Pro+ Scenarios

> **Plan:** $39/month → **3,900 AI Credits included**

### Scenario PP1: Active Developer — Mixed Workflow with Regular Agent Use

> The same "active developer" profile from Scenario P3/Business Scenario 12, now on Pro+

| | |
|--|--|
| **Monthly consumption** | **~1,918 credits** |
| **Pro+ plan included** | 3,900 credits |
| **% of plan used** | **49%** |
| **Headroom** | **~1,982 credits remaining** |

> ✅ Comfortable fit, with room for roughly one more agent-heavy day per week.

---

### Scenario PP2: Power User — Agent-Heavy Workflow

> Senior engineer relying heavily on agent mode for refactoring and feature work — same profile as Business Scenario 14

| # | Activity | Model | Credits |
|---|----------|-------|---------|
| 1–5 | 5 quick chats | GPT-5 mini | 0.5 |
| 6–8 | 3 agent sessions (medium, ~3 files each) | Claude Sonnet 4 | 75.0 |
| 9 | 1 agent session (large, 8+ files) | Claude Sonnet 4.6 | 40.0 |
| 10 | 1 architecture review | Claude Opus 4.7 | 85.0 |
| 11 | 1 code review | Auto-selected | 5.0 |
| | **Daily Total** | | **205.5 credits** |

| | |
|--|--|
| **Monthly (22 working days)** | **~4,521 credits** |
| **Pro+ plan included** | 3,900 credits |
| **Shortfall** | **~621 credits over allowance** |

> ⚠️ This power-user profile **exceeds Pro+ by ~16%**. Without pooling or overage, this developer would run out of credits roughly **3 working days before month end**. This is exactly the gap **Max+** is intended to close.

---

### Scenario PP3: Frontier-Model Heavy User

> Developer who deliberately favors the most capable (and most expensive) models for high-stakes work

| Model | Daily Uses | Credits Each | Daily Credits |
|-------|-----------|--------------|----------------|
| Claude Opus 4.7 (architecture reviews) | 1 | 85.0 | 85.0 |
| GPT-5.5 (complex generation) | 1 | 15.0 | 15.0 |
| Claude Sonnet 4.6 (agent sessions) | 2 | 40.0 | 80.0 |
| GPT-5 mini (routine chats) | 10 | 0.1 | 1.0 |
| | **Daily Total** | | **181.0 credits** |

| | |
|--|--|
| **Monthly (22 working days)** | **~3,982 credits** |
| **Pro+ plan included** | 3,900 credits |
| **Shortfall** | **~82 credits over allowance** |

> ⚠️ Marginal overage — a single lighter day (skip one Opus review) keeps this user within Pro+. Right at the boundary.

---

## 6. Copilot Max+ Scenarios (Projected)

> **Plan:** $79/month *(estimated)* → **7,900 AI Credits included** *(estimated)*
> All figures in this section are **projections**, not official GitHub pricing.

### Scenario M1: Power User — Agent-Heavy Workflow (Same as PP2)

| | |
|--|--|
| **Monthly consumption** | **~4,521 credits** |
| **Max+ plan included** *(est.)* | 7,900 credits |
| **% of plan used** | **57%** |
| **Headroom** | **~3,379 credits remaining** |

> ✅ The power-user profile that overflowed Pro+ by 16% fits comfortably in Max+, with capacity for nearly double the workload.

---

### Scenario M2: Daily Multi-Agent Workflow

> Developer running 3–4 substantial agent sessions per day across a large codebase

| # | Activity | Model | Credits |
|---|----------|-------|---------|
| 1–8 | 8 quick chats | GPT-5 mini | 0.8 |
| 9–11 | 3 agent sessions (medium, ~4 files each) | Claude Sonnet 4.6 | 90.0 |
| 12 | 1 agent session (large, 10+ files) | Claude Opus 4.5 | 110.0 |
| 13 | 1 architecture review | Claude Opus 4.7 | 85.0 |
| 14 | 2 code reviews | Auto-selected | 10.0 |
| | **Daily Total** | | **295.8 credits** |

| | |
|--|--|
| **Monthly (22 working days)** | **~6,508 credits** |
| **Max+ plan included** *(est.)* | 7,900 credits |
| **% of plan used** | **82%** |
| **Headroom** | **~1,392 credits (18%)** |

> ✅ Fits, but this is a genuinely heavy daily agent workload — the kind of usage that would blow well past both Pro (6.5x over) and Pro+ (1.7x over).

---

### Scenario M3: Extreme User — Continuous Agent Swarms

> Developer orchestrating parallel/background agent sessions most of the day (e.g., multiple concurrent coding agent tasks)

| # | Activity | Model | Credits |
|---|----------|-------|---------|
| 1–5 | 5 large agent sessions (8+ files each) | Claude Sonnet 4.6 | 200.0 |
| 6–7 | 2 architecture/design reviews | Claude Opus 4.7 | 170.0 |
| 8 | Miscellaneous chats & reviews | Mixed | 15.0 |
| | **Daily Total** | | **385.0 credits** |

| | |
|--|--|
| **Monthly (22 working days)** | **~8,470 credits** |
| **Max+ plan included** *(est.)* | 7,900 credits |
| **Shortfall** | **~570 credits over allowance** |

> ❌ Even the projected Max+ tier is exceeded by this extreme "always running agents" profile. This is the segment where **Business/Enterprise plans (pooled credits) or pay-as-you-go overage** become more cost-effective than any flat individual tier.

---

## 7. Cross-Plan Comparison

### Same Task, Same Model — Plan Headroom Comparison

> Task: "Fix the race condition in our WebSocket handler" (agent mode, ~24,000 tokens via Claude Sonnet 4.6, ≈10.65 credits)

| Plan | Included Credits | Credits per Task | Max Tasks/Month |
|------|-------------------|-------------------|------------------|
| Copilot Pro | 1,000 | 10.65 | ~93 |
| Copilot Pro+ | 3,900 | 10.65 | ~366 |
| Copilot Max+ *(est.)* | 7,900 | 10.65 | ~741 |

### Monthly Usage Profile vs. Plan Fit

| Developer Profile | Monthly Credits Needed | Fits Pro (1,000)? | Fits Pro+ (3,900)? | Fits Max+ (7,900, est.)? |
|--------------------|------------------------|:---:|:---:|:---:|
| Light user (chat only) | ~59–110 | ✅ | ✅ | ✅ |
| Moderate user | ~268–500 | ✅ | ✅ | ✅ |
| Active developer (mixed) | ~772–1,918 | ⚠️ tight / ❌ over | ✅ | ✅ |
| Power user (agent-heavy) | ~4,521 | ❌ | ❌ (~16% over) | ✅ |
| Daily multi-agent workflow | ~6,508 | ❌ | ❌ | ✅ |
| Extreme / continuous agent swarms | ~8,470+ | ❌ | ❌ | ⚠️ tight / ❌ over |

### Price-to-Credit Efficiency

| Plan | Price | Credits | $ per 1,000 credits |
|------|-------|---------|----------------------|
| Copilot Pro | $10 | 1,000 | **$10.00** |
| Copilot Pro+ | $39 | 3,900 | **$10.00** |
| Copilot Max+ *(est.)* | $79 | 7,900 | **$10.00** |

> All three tiers price out at the **same $10 per 1,000 credits** rate as Pro — consistent with Pro+'s existing ratio. The projected Max+ figure preserves this linear pricing rather than introducing a volume discount; GitHub could reasonably price an actual Max+ tier lower per-credit to incentivize upgrades.

---

## 8. Which Plan Fits Which Developer Profile

| If you... | Consider |
|-----------|----------|
| Ask occasional questions, rarely touch agent mode | **Copilot Pro** ($10/mo, 1,000 credits) |
| Chat daily and generate small snippets, no regular agent use | **Copilot Pro** ($10/mo) — with headroom to spare |
| Use agent mode a few times a week alongside daily chat | **Copilot Pro+** ($39/mo, 3,900 credits) |
| Run agent sessions daily, mix in frontier models regularly | **Copilot Pro+**, watch for overage near month-end |
| Run multiple agent sessions daily and lean on Opus/GPT-5.5-class models | **Copilot Max+** *(projected)* — or Business/Enterprise if available through work |
| Orchestrate continuous/parallel agent swarms most of the day | Even **Max+** may be tight — evaluate **Business/Enterprise pooling** or per-token overage instead |

---

## 9. What Happens When Credits Run Out

Individual plans (Pro, Pro+, and the projected Max+) do **not** support organization-style budgets, pooling, or (as far as currently documented) purchasing additional overage credits mid-cycle. When the included allowance is exhausted:

```
Day 1–N:   Normal usage draws down the monthly allowance
Day N:     Allowance reaches $0 / 0 credits
Day N–EOM: Chat, agent mode, and code review requiring AI Credits are blocked
           Code completions and Next Edit Suggestions keep working (always free)
Next cycle: Allowance resets to the plan's full included amount
```

- **No fallback to a lower-cost model** — this behavior was removed for all plans, individual and organizational, effective June 1, 2026
- **Code completions and Next Edit Suggestions remain unlimited and free** regardless of AI Credit balance
- **The only way to get more capacity mid-cycle today is to upgrade your plan** (Pro → Pro+, or Pro+ → the next tier when/if Max+ ships)
- If GitHub does release an official Max+ or an individual overage/top-up mechanism, update this document's [Section 2](#2-how-individual-plan-billing-works) accordingly

---

## Summary

- **Copilot Pro** ($10/mo, 1,000 credits) comfortably serves light-to-moderate chat users; active developers doing even occasional agent work will find it tight.
- **Copilot Pro+** ($39/mo, 3,900 credits) is the right fit for developers with regular but not constant agent-mode usage — roughly 4x the headroom of Pro.
- **Copilot Max+** *(projected, not yet announced — $79/mo, 7,900 credits estimated)* would extend that same linear scaling to cover daily, multi-session agent-heavy workflows that currently overflow Pro+ by double-digit percentages.
- Even a projected Max+ tier has a ceiling — developers running continuous/parallel agent swarms are better served by pooled Business/Enterprise plans, where light users' unused credits offset power users' overages.
