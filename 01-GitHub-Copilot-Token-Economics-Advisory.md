# GitHub Copilot Token Economics — Advisory Brief

> **Prepared:** April 29, 2026  
> **Effective Date of Change:** June 1, 2026  
> **Sources:** [GitHub Blog Announcement](https://github.blog/news-insights/company-news/github-copilot-is-moving-to-usage-based-billing/), [Usage-Based Billing Docs](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises), [Prepare for Billing Change](https://docs.github.com/en/copilot/how-tos/manage-and-track-spending/prepare-for-your-move-to-usage-based-billing), [Models & Pricing](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Why GitHub Is Making This Change](#2-why-github-is-making-this-change)
3. [What Exactly Is Changing on June 1](#3-what-exactly-is-changing-on-june-1)
4. [How GitHub AI Credits Work](#4-how-github-ai-credits-work)
5. [Model Pricing Reference](#5-model-pricing-reference)
6. [What This Means for Business & Enterprise](#6-what-this-means-for-business--enterprise)
7. [What This Means for Individuals](#7-what-this-means-for-individuals)
8. [Budget Controls & Governance](#8-budget-controls--governance)
9. [Monitoring & Dashboards](#9-monitoring--dashboards)
10. [Preparation Checklist](#10-preparation-checklist)
11. [FAQ with Answers](#11-faq-with-answers)

---

## 1. Executive Summary

Starting **June 1, 2026**, GitHub is replacing the current **Premium Request Unit (PRU)** billing model with **usage-based billing** measured in **GitHub AI Credits**. Credits are consumed based on actual token usage — input tokens, output tokens, and cached tokens — priced per model.

**Key facts:**
- Base plan pricing is **unchanged** ($19/user Business, $39/user Enterprise)
- Code completions and Next Edit Suggestions remain **unlimited and free** on paid plans
- Credits are **pooled** at the organization/enterprise level (not per-user silos)
- The fallback to a lower-cost model when quota is exhausted is **being removed**
- Copilot code review will also consume **GitHub Actions minutes**
- Enterprise/Business customers get a **3-month promotional** boost (June–August 2026)

---

## 2. Why GitHub Is Making This Change

Copilot has evolved from a simple in-editor autocomplete into an **agentic platform** capable of:
- Running long, multi-step autonomous coding sessions
- Using frontier-class AI models
- Iterating across entire repositories

Under the old model, a quick chat question and a multi-hour autonomous coding session cost the same. GitHub absorbed escalating inference costs, but the premium request model is **no longer sustainable**.

Usage-based billing:
- **Aligns pricing with actual usage** — you pay for what you consume
- **Improves service reliability** — reduces need to gate heavy users
- **Enables better budget controls** — granular visibility and spending caps

---

## 3. What Exactly Is Changing on June 1

| Aspect | Before June 1 (Current) | After June 1 (New) |
|--------|------------------------|---------------------|
| **Billing Unit** | Premium Request Units (PRUs) | GitHub AI Credits |
| **How Usage Is Measured** | 1 request per user prompt (flat) | Token-level: input + output + cached tokens |
| **Model Cost Differentiation** | Model multiplier (e.g., 1x, 3x, 7.5x) | Per-token rates vary by model |
| **Included Allowance** | Fixed PRU count per user per month | AI Credits in USD, pooled at org level |
| **Fallback When Exhausted** | Drop to included models (GPT-4.1, GPT-5 mini) | **No fallback** — usage governed by budget controls |
| **Code Completions** | Unlimited (paid plans) | Unchanged — still unlimited |
| **Code Review** | 1 premium request per review | AI Credits **+** GitHub Actions minutes |
| **Unused Allowance** | Does not roll over | Does not roll over |
| **Overage** | $0.04/premium request | Per-token rates at 1 AI Credit = $0.01 USD |

---

## 4. How GitHub AI Credits Work

### Delivery Mechanics: AI Credits

Tokens are measured, rated, and normalized into GitHub AI Credits.

> **1 AI Credit = $0.01**

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

### What Gets Counted

Three types of tokens are measured, rated, and billed:

```
┌─────────────────────────────────┐  ┌─────────────────────────────────┐  ┌─────────────────────────────────┐
│  ① Input Tokens                 │  │  ② Output Tokens                │  │  ③ Cache Read Tokens             │
│  "The prompt"                   │  │  "The result"                   │  │  "The memory"                   │
│                                 │  │                                 │  │                                 │
│  Prompt + context sent to       │  │  Model response                 │  │  Reused cached content           │
│  the model                      │  │                                 │  │  that lowers cost                │
│                                 │  │                                 │  │                                 │
│  • Prompts                      │  │  • Generated code               │  │  • Repeated unchanged context    │
│  • Instructions                 │  │  • Explanations                 │  │  • Reusable instructions         │
│  • Code, files, docs            │  │  • Docs                         │  │  • Ongoing conversation context  │
│  • Previous conversation        │  │  • Recommendations              │  │                                 │
│  • Tool/MCP context             │  │  • Large refactors              │  │                                 │
└─────────────────────────────────┘  └─────────────────────────────────┘  └─────────────────────────────────┘
  Base rate                           Most expensive (4–10× input)         ~10× cheaper than fresh input
```

| Token Type | What It Is | What It Includes | Relative Cost |
|-----------|-----------|-----------------|---------------|
| **① Input Tokens** | The prompt — what you send to the model | Prompts, instructions, code, files, docs, previous conversation, tool/MCP context | Base rate |
| **② Output Tokens** | The result — what the model generates | Generated code, explanations, docs, recommendations, large refactors | **Most expensive** (4–10× input) |
| **③ Cache Read Tokens** | The memory — reused cached content that lowers cost | Repeated unchanged context, reusable instructions, ongoing conversation context | **~10× cheaper** than fresh input |
| **Cache Write** (Anthropic only) | Cost to write content into cache | New context stored for future reuse | Additional charge |

### Key Rules

- **Rates vary by model and token type** — each model has its own input, cache, and output rate
- **Tokens are not interchangeable** — input tokens, cache read tokens, and output tokens are priced separately
- **Code completions and Next Edit suggestions remain included** — they do not consume AI Credits
- **Copilot Code Review consumes AI Credits and GitHub Actions minutes** — dual billing

### Credit Conversion

- **1 AI Credit = $0.01 USD**
- A $10 budget = 1,000 AI credits
- A $19/user Business plan includes $19 in AI credits = **1,900 AI credits/user/month**

### Pooling

Credits are **pooled at the billing entity level**:
- Enterprise with 100 Business users → **190,000 shared credits** (not 100 × 1,900 individual buckets)
- Power users can draw more; lighter users offset
- Adding licenses mid-cycle **increases** the pool immediately
- Removing licenses mid-cycle does **not** shrink the pool until next cycle

---

## 5. Model Pricing Reference

> All prices are **per 1 million tokens**. 1 AI Credit = $0.01 USD.

### OpenAI Models

| Model | Tier | Input | Cached Input | Output |
|-------|------|-------|-------------|--------|
| **GPT-5 mini** ★ | Lightweight | $0.25 | $0.025 | $2.00 |
| **GPT-4.1** ★ | Versatile | $2.00 | $0.50 | $8.00 |
| GPT-5.2 | Versatile | $1.75 | $0.175 | $14.00 |
| GPT-5.2-Codex | Powerful | $1.75 | $0.175 | $14.00 |
| GPT-5.3-Codex | Powerful | $1.75 | $0.175 | $14.00 |
| GPT-5.4 | Versatile | $2.50 | $0.25 | $15.00 |
| GPT-5.4 mini | Lightweight | $0.75 | $0.075 | $4.50 |
| GPT-5.4 nano | Lightweight | $0.20 | $0.02 | $1.25 |
| GPT-5.5 | Powerful | $5.00 | $0.50 | $30.00 |

> ★ **Included models** — GPT-4.1 and GPT-5 mini do **not** consume AI Credits under the current PRU model (paid plans). Under the new model, they will consume AI Credits based on token usage.

### Anthropic Models

| Model | Tier | Input | Cached Input | Cache Write | Output |
|-------|------|-------|-------------|-------------|--------|
| Claude Haiku 4.5 | Versatile | $1.00 | $0.10 | $1.25 | $5.00 |
| Claude Sonnet 4 / 4.5 / 4.6 | Versatile | $3.00 | $0.30 | $3.75 | $15.00 |
| Claude Opus 4.5 / 4.6 | Powerful | $5.00 | $0.50 | $6.25 | $25.00 |
| Claude Opus 4.7 | Powerful | $5.00 | $0.50 | $6.25 | $25.00 |

### Google Models

| Model | Tier | Input | Cached Input | Output |
|-------|------|-------|-------------|--------|
| Gemini 2.5 Pro | Powerful | $1.25 | $0.125 | $10.00 |
| Gemini 3 Flash | Lightweight | $0.50 | $0.05 | $3.00 |
| Gemini 3.1 Pro | Powerful | $2.00 | $0.20 | $12.00 |

### Other Models

| Model | Tier | Input | Cached Input | Output |
|-------|------|-------|-------------|--------|
| Grok Code Fast 1 | Lightweight | $0.20 | $0.02 | $1.50 |
| Raptor mini | Versatile | $0.25 | $0.025 | $2.00 |
| Goldeneye | Powerful | $1.25 | $0.125 | $10.00 |

### Cost Comparison: Cheapest vs. Most Expensive

| | Cheapest (GPT-5.4 nano) | Most Expensive (GPT-5.5) |
|--|-------------------------|--------------------------|
| Input per 1M tokens | $0.20 | $5.00 |
| Output per 1M tokens | $1.25 | $30.00 |
| **Ratio** | **1x** | **24x output cost** |

---

## 6. What This Means for Business & Enterprise

### Pricing (Unchanged)

| Plan | Monthly Seat Price | Included AI Credits | Included $ Value |
|------|-------------------|--------------------|-|
| **Copilot Business** | $19/user/month | 1,900 credits/user | $19/user |
| **Copilot Enterprise** | $39/user/month | 3,900 credits/user | $39/user |

### Promotional Credits (June–August 2026)

Existing Business and Enterprise customers receive elevated included usage for the first 3 months:

| Plan | Standard Credits/User | Promotional Credits/User | Promotional $ Value |
|------|----------------------|-------------------------|---------------------|
| Copilot Business | 1,900 | **3,000** | $30/user |
| Copilot Enterprise | 3,900 | **7,000** | $70/user |

### Overage Pricing

When the pooled credit pool is exhausted:
- **If additional usage is allowed:** Billed at published per-token rates (1 AI Credit = $0.01)
- **If additional usage is blocked:** Access halted until next billing cycle
- **User-level budget exhausted:** That user's access stops regardless of pool capacity

### Key Enterprise Changes

1. **No more fallback models** — today users who exhaust PRUs fall back to GPT-4.1/GPT-5 mini. After June 1, usage is governed by credit availability and budget controls only.
2. **Code review = AI Credits + Actions minutes** — Copilot code review will consume both token-based AI credits AND GitHub Actions minutes on GitHub-hosted runners.
3. **Pooled credits** — eliminates stranded per-user capacity; power users benefit from light users' unused allocation.
4. **Data residency/FedRAMP** — adds a 10% model multiplier increase for compliant requests.

---

## 7. What This Means for Individuals

| Plan | Monthly Price | Included AI Credits |
|------|--------------|---------------------|
| Copilot Free | $0 | Limited (50 premium requests/month under current model) |
| Copilot Pro | $10/month | $10 in AI Credits |
| Copilot Pro+ | $39/month | $39 in AI Credits |

- **Monthly subscribers** auto-migrate to usage-based billing on June 1
- **Annual subscribers** remain on current PRU model until plan expiration, then transition to Copilot Free (can upgrade to monthly paid plan anytime)
- Annual subscribers see **increased model multipliers on June 1** (e.g., Claude Opus goes from 3x → 27x multiplier)

---

## 8. Budget Controls & Governance

### Four-Level Budget Hierarchy

| Level | Scope | Who Sets It |
|-------|-------|-------------|
| **Enterprise** | All orgs, repos, cost centers | Enterprise owner/billing manager |
| **Organization** | All repos in the org | Org owner/billing manager |
| **Cost Center** | Logical groupings (BU/team/project) | Enterprise owner |
| **User** | Individual developer cap | Org/enterprise owner |

### Budget Behaviors

- Budgets can trigger **alerts** at 75%, 90%, 100% thresholds
- Budgets can enforce **hard stops** on usage
- A $0 user-level budget = **no access at all**
- Budgets are set in **USD**; usage draws down at 1 AI Credit = $0.01
- If a user exhausts their personal budget, their access halts **even if the org pool has capacity**
- No automatic fallback to lower-cost models when budget is exhausted

### Policy Controls

Enterprise owners can set:
- Whether users can incur **additional usage** beyond included allowance
- Per-user budget caps
- Model availability (which models are enabled/disabled)

---

## 9. Monitoring & Dashboards

### Available Monitoring Tools

| Tool | Access | What It Shows |
|------|--------|---------------|
| **IDE Status Bar** (VS Code, JetBrains, etc.) | All users | Real-time personal consumption, approaching limits |
| **Billing Overview** (github.com/settings/billing) | All users | Metered usage summary → Copilot breakdown |
| **Premium Request Analytics** | Enterprise/Org owners | Filter by model, user, timeframe; downloadable charts |
| **Usage Reports (CSV)** | Billing managers | Per-user, per-model, per-day granularity with `aic_quantity` and `aic_gross_amount` columns |
| **Billing Preview Tool** (coming early May) | Admins | Interactive estimated costs broken down by model, user, SKU |
| **REST API** | Enterprise owners | Programmatic access to usage analytics |

### Usage Report Columns (New)

| Column | Description |
|--------|-------------|
| `aic_quantity` | Number of AI credits consumed |
| `aic_gross_amount` | Estimated cost in USD under usage-based billing |

---

## 10. Preparation Checklist

### Data Availability Calendar

> Use data moments to prioritize customers before GA.

```
┌─────────────────────────────┐  ┌─────────────────────────────┐  ┌─────────────────────────────┐
│  Apr 27                     │  │  Early May                  │  │  May                        │
│                             │  │                             │  │                             │
│  Announcement +             │  │  Preview bill experience    │  │  Usage CSV / sidecar        │
│  portal assets              │  │                             │  │                             │
│                             │  │  Identify high-impact       │  │  Analyze model, user,       │
│  Use approved terminology   │  │  accounts                   │  │  request-based spend        │
│  and assets                 │  │                             │  │                             │
└─────────────────────────────┘  └─────────────────────────────┘  └─────────────────────────────┘
┌─────────────────────────────┐  ┌─────────────────────────────┐  ┌─────────────────────────────┐
│  May 6/7                    │  │  Jun 1                      │  │  Jun–Aug                    │
│                             │  │                             │  │                             │
│  Office hours               │  │  UBB GA                     │  │  Promo included usage       │
│                             │  │                             │  │                             │
│  Close product and          │  │  Budgets and user-level     │  │  Stabilize before standard  │
│  reporting questions        │  │  limits operational         │  │  usage resumes              │
│                             │  │                             │  │                             │
└─────────────────────────────┘  └─────────────────────────────┘  └─────────────────────────────┘
```

| Date | Milestone | Action |
|------|-----------|--------|
| **Apr 27** | Announcement + portal assets | Use approved terminology and assets |
| **Early May** | Preview bill experience | Identify high-impact accounts |
| **May** | Usage CSV / sidecar | Analyze model, user, request-based spend |
| **May 6/7** | Office hours | Close product and reporting questions |
| **Jun 1** | **UBB GA** | Budgets and user-level limits operational |
| **Jun–Aug** | Promo included usage | Stabilize before standard usage resumes |

### Optimization Levers

> Reduce waste first. Do not suppress high-value adoption.

| Lever | What It Means | Guidance |
|-------|--------------|----------|
| **Auto mode** | Route by task intent | Avoid defaulting every task to frontier models |
| **Context hygiene** | Keep context clean | Compact, summarize, or start a new chat when context diverges |
| **Prompt scoping** | Constrain and stage | Ask for constrained, staged outputs instead of one giant request |
| **Agent discipline** | High-value workflows only | Use agents for high-value workflows; avoid fleets of agents for trivial work |
| **Tool / MCP hygiene** | Enable selectively | Enable tools only when needed; broad tool context can increase usage |
| **User limits** | Cap aggressive consumption | Cap aggressive consumption while preserving pilot exceptions |

### ✅ Before June 1 — Actions for Enterprise/Org Admins

- [ ] **Preview your bill** — Use the billing preview tool from the enterprise home page banner to compare current vs. projected costs
- [ ] **Download usage report** — Get the CSV with `aic_quantity` and `aic_gross_amount` to understand per-user, per-model consumption
- [ ] **Identify top consumers** — Analyze usage by user and model to find outliers
- [ ] **Review existing budgets** — Enterprise-level PRU budgets auto-carry-over; verify limits still make sense
- [ ] **Set up 4-level budget structure** — Enterprise → Org → Cost Center → User budgets
- [ ] **Define overage policy** — Decide: allow additional usage at published rates, or hard-stop at pool limit?
- [ ] **Establish model routing policy** — Which models are available to which teams?
- [ ] **Communicate changes internally** — Brief engineering teams on what's changing and what it means
- [ ] **Plan for pooled credits** — Understand how uneven usage patterns change cost structures
- [ ] **Prepare for no-fallback world** — Users can no longer fall back to included models when exhausted
- [ ] **Review code review setup** — Copilot code review will now consume GitHub Actions minutes; configure runners appropriately
- [ ] **Create developer education materials** — Model selection cheat sheets, prompt optimization guides

---

## 11. FAQ with Answers

### General

**Q: Are base plan prices increasing?**  
A: No. Copilot Business remains $19/user/month and Enterprise remains $39/user/month. What changes is how usage within and beyond the included allowance is measured.

**Q: What is a GitHub AI Credit?**  
A: The new billing unit for Copilot usage. 1 AI Credit = $0.01 USD. When you use Copilot, the interaction consumes tokens (input, output, cached), and the total token cost is converted into AI Credits.

**Q: What are tokens?**  
A: Tokens are the fundamental units of text that AI models process. Roughly, 1 token ≈ 4 characters or ¾ of a word. Every interaction has input tokens (your prompt + context), output tokens (the model's response), and potentially cached tokens (reused context).

**Q: When does this take effect?**  
A: June 1, 2026 for all monthly Copilot plans. Annual individual subscribers transition when their plan expires.

**Q: Do code completions and Next Edit Suggestions cost AI Credits?**  
A: No. Code completions and next edit suggestions remain **unlimited and free** on all paid plans.

### Billing & Credits

**Q: How are included AI credits calculated?**  
A: Each assigned license contributes credits to a shared pool. Business = 1,900 credits/user/month ($19), Enterprise = 3,900 credits/user/month ($39).

**Q: Are credits pooled or per-user?**  
A: **Pooled** at the billing entity level. 100 Business users = 190,000 shared credits. Power users can draw more; lighter users offset.

**Q: Do unused credits roll over?**  
A: No. Unused credits reset at the start of each billing cycle.

**Q: What happens when credits run out?**  
A: It depends on your admin's policy:  
- **Additional usage allowed:** Usage continues at published per-token rates  
- **Additional usage not allowed:** Usage is blocked until next billing cycle  
- There is **no automatic fallback** to cheaper models

**Q: Can I still fall back to GPT-4.1 or GPT-5 mini when I run out?**  
A: No. The fallback experience is being removed. After June 1, usage is governed entirely by available credits and budget controls.

**Q: What is the overage rate?**  
A: Additional usage is billed at the per-token rates for each model, converted to AI Credits at 1 Credit = $0.01 USD. The exact cost depends on which model and how many tokens are consumed.

**Q: Is there a promotional period?**  
A: Yes. Existing Business and Enterprise customers receive elevated credits for June, July, and August 2026:  
- Business: 3,000 credits/user (vs. standard 1,900)  
- Enterprise: 7,000 credits/user (vs. standard 3,900)

### Cost Management

**Q: How can I set spending limits?**  
A: Budgets can be set at four levels: Enterprise, Organization, Cost Center, and User. These can trigger alerts and/or enforce hard stops.

**Q: If I set a user budget and the user exceeds it, can they still use Copilot from the org pool?**  
A: No. If a user exhausts their individual budget, their access is halted regardless of whether the org pool still has capacity.

**Q: Where do I monitor usage?**  
A: Multiple places: IDE status bar (real-time), Billing overview page, Premium request analytics page, downloadable CSV usage reports, and via the REST API.

**Q: What is the billing preview tool?**  
A: Available from early May 2026, it shows a side-by-side comparison of your actual spend under the current model vs. estimated spend under usage-based billing, based on your April 2026 usage.

### Models & Features

**Q: Which models are cheapest?**  
A: GPT-5.4 nano ($0.20 input/$1.25 output per 1M tokens), Grok Code Fast 1 ($0.20/$1.50), and GPT-5 mini ($0.25/$2.00).

**Q: Which models are most expensive?**  
A: GPT-5.5 ($5.00 input/$30.00 output per 1M tokens), Claude Opus 4.5/4.6/4.7 ($5.00/$25.00), and GPT-5.4 for large prompts ($2.50/$15.00).

**Q: Does Copilot code review cost more now?**  
A: Yes. Code review now consumes **both** AI Credits (token-based) **and** GitHub Actions minutes. The Actions minutes are billed at standard GitHub-hosted runner rates.

**Q: What about data residency or FedRAMP?**  
A: Requests processed with data residency or FedRAMP enforcement include an additional **10% multiplier** on costs.

**Q: Which Copilot features consume AI Credits?**  
A: Copilot Chat, Copilot CLI, Copilot Cloud Agent, Copilot Spaces, Spark, third-party coding agents, and Copilot code review. **Not billed:** Code completions and Next Edit Suggestions.

**Q: Does auto model selection give a discount?**  
A: Under the current PRU model, auto model selection provides a 10% multiplier discount in VS Code. Pricing under the new model is per-token.

### Transition

**Q: Do I need to do anything to switch to the new billing?**  
A: Monthly plans will **auto-migrate** on June 1. No action needed for the switch itself. However, you should proactively prepare budgets, review usage, and communicate changes.

**Q: What happens to my existing premium request budgets?**  
A: Enterprise-level budgets for premium requests will **automatically carry over** to AI Credits. Review them to ensure they still reflect your desired limits.

**Q: Where can I preview my projected costs?**  
A: From the announcement banner on your enterprise home page, billing overview page, or premium request analytics page, click "Preview my bill" (available early May 2026).

---

*Last updated: April 29, 2026*
