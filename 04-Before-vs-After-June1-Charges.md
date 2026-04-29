# Token Charges: Before vs. After June 1, 2026

> **Date:** April 29, 2026  
> **Purpose:** Side-by-side comparison of GitHub Copilot billing before and after the transition to usage-based billing

---

## Table of Contents

1. [Plan Pricing Comparison](#1-plan-pricing-comparison)
2. [How Usage Is Measured — Before vs. After](#2-how-usage-is-measured--before-vs-after)
3. [Model Costs — Before vs. After](#3-model-costs--before-vs-after)
4. [Feature-by-Feature Billing Comparison](#4-feature-by-feature-billing-comparison)
5. [Overage & Exhaustion Behavior](#5-overage--exhaustion-behavior)
6. [Budget Controls Comparison](#6-budget-controls-comparison)
7. [Real-World Cost Scenarios — Before vs. After](#7-real-world-cost-scenarios--before-vs-after)
8. [Key Differences Summary](#8-key-differences-summary)
9. [What Gets Cheaper, What Gets More Expensive](#9-what-gets-cheaper-what-gets-more-expensive)
10. [Transition Timeline](#10-transition-timeline)

---

## 1. Plan Pricing Comparison

### Base Seat Prices (Unchanged)

| Plan | Monthly Price | Before June 1 | After June 1 |
|------|--------------|---------------|--------------|
| Copilot Free | $0 | 50 premium requests/month | Limited AI Credits |
| Copilot Pro | $10/month | 300 premium requests/month | $10 in AI Credits (1,000 credits) |
| Copilot Pro+ | $39/month | 1,500 premium requests/month | $39 in AI Credits (3,900 credits) |
| **Copilot Business** | **$19/user/month** | 300 premium requests/user/month | $19 in AI Credits/user (1,900 credits) **pooled** |
| **Copilot Enterprise** | **$39/user/month** | 1,000 premium requests/user/month | $39 in AI Credits/user (3,900 credits) **pooled** |

### Promotional Period (Existing Business & Enterprise Customers)

| Plan | Standard After June 1 | Promo (June–August 2026) |
|------|----------------------|--------------------------|
| Copilot Business | 1,900 credits/user ($19) | **3,000 credits/user ($30)** |
| Copilot Enterprise | 3,900 credits/user ($39) | **7,000 credits/user ($70)** |

---

## 2. How Usage Is Measured — Before vs. After

### What Gets Counted (After June 1)

Under usage-based billing, three types of tokens are measured, rated, and billed:

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

> **Before June 1:** None of this mattered — every interaction was a flat premium request regardless of token count.  
> **After June 1:** Every token type is measured and priced separately per model.

### Before June 1: Premium Request Units (PRUs)

```
1 user prompt = 1 premium request × model multiplier

Examples:
- Chat with GPT-4.1 (included model)    → 0 premium requests (free)
- Chat with GPT-5 mini (included model)  → 0 premium requests (free)
- Chat with Claude Sonnet 4              → 1 premium request (1x multiplier)
- Chat with Claude Opus 4.7              → 7.5 premium requests (7.5x multiplier)
- Chat with GPT-5.5                      → 7.5 premium requests (7.5x multiplier)
- Chat with Claude Haiku 4.5             → 0.33 premium requests
- Spark prompt                           → 4 premium requests (fixed)
- Cloud agent session                    → 1 request × model multiplier per prompt
- Code review                            → 1 premium request per review
```

**Key characteristic:** Cost is **flat per interaction** regardless of prompt length, context size, or response length.

### After June 1: GitHub AI Credits (Token-Based)

```
Cost = (input_tokens × input_rate) + (cached_tokens × cached_rate) + (output_tokens × output_rate)
Then converted to AI Credits at 1 credit = $0.01

Examples (approximate):
- Short chat with GPT-5 mini (500 in, 300 out)
  = (500 × $0.25/1M) + (300 × $2.00/1M) = $0.000725 → ~0.07 credits

- Complex chat with Claude Sonnet 4 (8,000 in, 4,000 out)
  = (8,000 × $3.00/1M) + (4,000 × $15.00/1M) = $0.084 → ~8.4 credits

- Agent session with Claude Opus 4.7 (80,000 in, 15,000 out)
  = (80,000 × $5.00/1M) + (15,000 × $25.00/1M) = $0.775 → ~77.5 credits
```

**Key characteristic:** Cost is **proportional to actual token consumption** — longer prompts, bigger context, and longer responses cost more.

---

## 3. Model Costs — Before vs. After

### Current Model Multipliers (Before June 1)

| Model | Multiplier (Paid Plans) | Cost per Chat Interaction |
|-------|------------------------|--------------------------|
| GPT-4.1 | **0** (included) | Free |
| GPT-5 mini | **0** (included) | Free |
| Raptor mini | **0** (included) | Free |
| Claude Haiku 4.5 | 0.33 | 0.33 premium requests |
| GPT-5.4 nano | 0.25 | 0.25 premium requests |
| Grok Code Fast 1 | 0.25 | 0.25 premium requests |
| Claude Sonnet 4 | 1 | 1 premium request |
| Claude Sonnet 4.5 | 1 | 1 premium request |
| Claude Sonnet 4.6 | 1 | 1 premium request |
| Gemini 2.5 Pro | 1 | 1 premium request |
| GPT-5.2 | 1 | 1 premium request |
| GPT-5.2-Codex | 1 | 1 premium request |
| Claude Opus 4.5 | 3 | 3 premium requests |
| Claude Opus 4.6 | 3 | 3 premium requests |
| Claude Opus 4.7 | **7.5** | 7.5 premium requests |
| GPT-5.5 | **7.5** | 7.5 premium requests |

### New Per-Token Pricing (After June 1)

| Model | Input/1M Tokens | Cached/1M | Output/1M Tokens | Approx. Credits for Typical Chat* |
|-------|----------------|-----------|------------------|----------------------------------|
| GPT-5 mini | $0.25 | $0.025 | $2.00 | ~0.07 |
| GPT-4.1 | $2.00 | $0.50 | $8.00 | ~0.5 |
| Raptor mini | $0.25 | $0.025 | $2.00 | ~0.07 |
| Claude Haiku 4.5 | $1.00 | $0.10 | $5.00 | ~0.3 |
| GPT-5.4 nano | $0.20 | $0.02 | $1.25 | ~0.05 |
| Grok Code Fast 1 | $0.20 | $0.02 | $1.50 | ~0.06 |
| Claude Sonnet 4 | $3.00 | $0.30 | $15.00 | ~8.4 |
| Gemini 2.5 Pro | $1.25 | $0.125 | $10.00 | ~4.5 |
| GPT-5.2 | $1.75 | $0.175 | $14.00 | ~6.5 |
| Claude Opus 4.6 | $5.00 | $0.50 | $25.00 | ~12.5 |
| Claude Opus 4.7 | $5.00 | $0.50 | $25.00 | ~12.5 |
| GPT-5.5 | $5.00 | $0.50 | $30.00 | ~14.0 |

> *Typical chat: ~2,000 input tokens, ~1,000 output tokens

### Critical Change: Included Models Now Cost Credits

| Model | Before June 1 | After June 1 |
|-------|---------------|--------------|
| GPT-4.1 | **Free** (0 premium requests) | **Costs credits** ($2.00 input/$8.00 output per 1M) |
| GPT-5 mini | **Free** (0 premium requests) | **Costs credits** ($0.25 input/$2.00 output per 1M) |
| Raptor mini | **Free** (0 premium requests) | **Costs credits** ($0.25 input/$2.00 output per 1M) |

**This is the most significant change for heavy users of "included" models.** Under the current model, unlimited chat with GPT-4.1 is free. After June 1, every token counts.

---

## 4. Feature-by-Feature Billing Comparison

| Feature | Before June 1 | After June 1 |
|---------|---------------|--------------|
| **Code Completions** | Unlimited, free (paid plans) | **Unchanged** — still unlimited, free |
| **Next Edit Suggestions** | Unlimited, free (paid plans) | **Unchanged** — still unlimited, free |
| **Copilot Chat** | 1 premium request × model multiplier per prompt | AI Credits based on tokens consumed |
| **Copilot Chat (included models)** | **Free** | **Costs AI Credits** (token-based) |
| **Agent Mode** | 1 premium request per user prompt × multiplier | AI Credits based on all tokens (including tool calls) |
| **Copilot CLI** | 1 premium request per prompt × multiplier | AI Credits based on tokens |
| **Copilot Cloud Agent** | 1 premium request per prompt × multiplier | AI Credits based on full session tokens |
| **Copilot Code Review** | 1 premium request per review | AI Credits **+** GitHub Actions minutes |
| **Copilot Spaces** | 1 premium request per prompt × multiplier | AI Credits based on tokens |
| **Spark** | Fixed 4 premium requests per prompt | AI Credits based on tokens |
| **Third-party coding agents** | 1 premium request per prompt | AI Credits based on tokens |

---

## 5. Overage & Exhaustion Behavior

### Before June 1 (Current Behavior)

```
User exhausts premium requests
    ↓
Fallback to included models (GPT-4.1, GPT-5 mini)
    ↓
Can continue working with reduced model capability
    ↓
OR: If enterprise allows, purchase additional at $0.04/request
```

### After June 1 (New Behavior)

```
User/org exhausts AI Credits pool
    ↓
IF "Additional usage allowed" by admin:
    → Continue at per-token rates (1 credit = $0.01)
    ↓
IF "Additional usage NOT allowed" by admin:
    → Usage BLOCKED until next billing cycle
    → No fallback to cheaper models
    ↓
IF user-level budget exhausted:
    → User BLOCKED even if org pool has credits remaining
```

### Key Differences in Exhaustion

| Aspect | Before June 1 | After June 1 |
|--------|---------------|--------------|
| Fallback to cheap models | ✅ Yes (GPT-4.1, GPT-5 mini) | ❌ **No fallback** |
| Overage rate | $0.04 per premium request (flat) | Per-token rates (variable by model) |
| User can keep working when exhausted | ✅ Yes (with included models) | ❌ Only if budget allows overages |
| Admin control granularity | Enterprise/org level | Enterprise/org/cost-center/**user** level |

---

## 6. Budget Controls Comparison

### Before June 1

| Control | Available? |
|---------|-----------|
| Enterprise budget | ✅ Yes (for premium request overage) |
| Organization budget | ✅ Yes |
| Cost center budget | ❌ No |
| User-level budget | ❌ No |
| Budget alerts | ✅ Yes (75%, 90%, 100%) |
| Hard spending caps | ✅ Yes |
| Policy: allow/block overages | ✅ Yes |

### After June 1

| Control | Available? |
|---------|-----------|
| Enterprise budget | ✅ Yes (for AI Credits) |
| Organization budget | ✅ Yes |
| **Cost center budget** | ✅ **Yes (NEW)** |
| **User-level budget** | ✅ **Yes (NEW)** |
| Budget alerts | ✅ Yes (configurable thresholds) |
| Hard spending caps | ✅ Yes |
| Policy: allow/block overages | ✅ Yes |
| **$0 user budget = no access** | ✅ **Yes (NEW)** |

---

## 7. Real-World Cost Scenarios — Before vs. After

### Scenario A: Developer Using Only Included Models (Chat Heavy)

> 100 chat interactions/day with GPT-4.1, 22 working days/month

**Before June 1:**
| Item | Calculation | Cost |
|------|------------|------|
| 2,200 chats with GPT-4.1 | 0 PRUs each (included model) | **$0 (free)** |
| Premium requests used | 0 | |

**After June 1:**
| Item | Calculation | Cost |
|------|------------|------|
| 2,200 chats × ~1,400 tokens avg | ~3.08M total tokens | |
| Input (2,000 tokens × 2,200) | 4.4M tokens × $2.00/1M | $8.80 |
| Output (1,000 tokens × 2,200) | 2.2M tokens × $8.00/1M | $17.60 |
| **Total** | | **$26.40 (2,640 credits)** |

> ⚠️ **This user goes from $0 to ~$26/month.** This is the biggest impact area — heavy users of previously-free models now incur costs. However, Business plan includes $19 in credits, and pooling may absorb the overage.

### Scenario B: Developer Using Premium Models Moderately

> 20 chats/day with Claude Sonnet 4, 22 working days/month

**Before June 1:**
| Item | Calculation | Cost |
|------|------------|------|
| 440 chats with Sonnet 4 | 1 PRU each = 440 PRUs | |
| Business plan includes | 300 PRUs | |
| Overage | 140 PRUs × $0.04 | **$5.60 overage** |

**After June 1:**
| Item | Calculation | Cost |
|------|------------|------|
| Input (3,000 tokens × 440) | 1.32M tokens × $3.00/1M | $3.96 |
| Output (1,500 tokens × 440) | 0.66M tokens × $15.00/1M | $9.90 |
| **Total** | | **$13.86 (1,386 credits)** |
| Business plan includes | 1,900 credits ($19) | |
| **Overage** | | **$0 (within budget)** |

> ✅ **This user goes from $5.60 overage to $0 overage** — moderate premium model users may benefit.

### Scenario C: Agent-Heavy Developer

> 5 agent sessions/day using Claude Sonnet 4.6, each session ~50K input + 20K output tokens

**Before June 1:**
| Item | Calculation | Cost |
|------|------------|------|
| 110 agent sessions/month | ~2 user prompts per session × 1 PRU each | ~220 PRUs |
| Business plan includes | 300 PRUs | |
| **Overage** | | **$0 (within budget)** |

**After June 1:**
| Item | Calculation | Cost |
|------|------------|------|
| Input (50K tokens × 110) | 5.5M tokens × $3.00/1M | $16.50 |
| Output (20K tokens × 110) | 2.2M tokens × $15.00/1M | $33.00 |
| **Total** | | **$49.50 (4,950 credits)** |
| Business plan includes | 1,900 credits ($19) | |
| **Overage** | 3,050 credits | **$30.50 overage** |

> ⚠️ **This user goes from $0 overage to $30.50/month overage.** Agent-heavy workflows are the most impacted because the current PRU model doesn't account for token volume. Pooling may help if other team members use fewer credits.

### Scenario D: Team of 20 — Mixed Usage

**Before June 1:**
| Profile | Users | PRUs/User/Month | Total PRUs | Overage |
|---------|-------|----------------|------------|---------|
| Heavy (agent) | 3 | 500 | 1,500 | |
| Moderate (premium chat) | 10 | 200 | 2,000 | |
| Light (included models) | 7 | 0 | 0 | |
| **Total** | **20** | | **3,500** | |
| Included (20 × 300) | | | 6,000 | |
| **Overage** | | | | **$0** |

**After June 1:**
| Profile | Users | Credits/User/Month | Total Credits |
|---------|-------|--------------------|---------------|
| Heavy (agent) | 3 | 4,950 | 14,850 |
| Moderate (premium chat) | 10 | 1,386 | 13,860 |
| Light (included models) | 7 | 300 | 2,100 |
| **Total** | **20** | | **30,810** |
| Included Pool (20 × 1,900) | | | 38,000 |
| **Remaining** | | | **7,190 credits** |

**During Promo (June–Aug):**
| Included Pool (20 × 3,000) | | | 60,000 |
| **Remaining** | | | **29,190 credits** |

> ✅ **Pooling saves this team.** Even with 3 heavy agent users, the team stays within budget because 7 light users contribute unused credits to the pool.

---

## 8. Key Differences Summary

| Dimension | Before June 1 | After June 1 |
|-----------|---------------|--------------|
| **Billing unit** | Premium Request Units (PRU) | GitHub AI Credits (token-based) |
| **Measurement** | Flat per interaction | Per token (input + cached + output) |
| **Included models (GPT-4.1, GPT-5 mini)** | **Free, unlimited** | **Consume AI Credits** |
| **Cost scales with** | Model choice only | Model choice × prompt size × response length |
| **Allowance structure** | Per-user fixed count | **Pooled** across org/enterprise |
| **Fallback when exhausted** | Drop to included models | **No fallback** |
| **Overage rate** | $0.04/PRU (flat) | Per-token rates (variable) |
| **Code completions** | Free | Free (unchanged) |
| **Code review** | 1 PRU per review | AI Credits **+ Actions minutes** |
| **Budget granularity** | Enterprise/org | Enterprise/org/**cost center/user** |
| **Unused allowance** | Does not roll over | Does not roll over |
| **Visibility** | PRU count | Token-level breakdown by model |

---

## 9. What Gets Cheaper, What Gets More Expensive

### Likely Cheaper After June 1

| Activity | Why |
|----------|-----|
| Short, targeted chats with any model | Token-based pricing rewards concise interactions |
| Light/occasional Copilot users | Pooling lets their credits go to power users |
| Teams with uneven usage | Pooling eliminates stranded per-user capacity |
| Small prompts to premium models | Were 1+ PRU flat; now priced by actual (small) token count |
| Using lightweight models (GPT-5.4 nano, Grok Fast) | Very low per-token rates |

### Likely More Expensive After June 1

| Activity | Why |
|----------|-----|
| **Heavy use of GPT-4.1 / GPT-5 mini** | Were completely free; now cost credits |
| Long agent sessions with large context | Token volume is high; PRU model didn't reflect this |
| Sending large files/repos as context | Input tokens add up; flat PRU model ignored context size |
| Multiple regenerations of same prompt | Each attempt consumes tokens; PRU model charged same per attempt |
| Copilot code review | Now has dual cost: AI Credits + Actions minutes |
| Frontier models with long outputs | Output tokens are the most expensive component |

### Net Neutral

| Activity | Why |
|----------|-----|
| Code completions | Free before and after |
| Next Edit Suggestions | Free before and after |
| Base plan prices | Unchanged |

---

## 10. Transition Timeline

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

### Detailed Timeline

| Date | Event |
|------|-------|
| **April 27, 2026** | GitHub announces usage-based billing transition |
| **Early May 2026** | Billing preview tool available — compare current vs. projected costs |
| **May 2026** | Download usage reports with `aic_quantity` and `aic_gross_amount` columns |
| **May 6/7, 2026** | Office hours — close product and reporting questions |
| **June 1, 2026** | **Usage-based billing takes effect** for all monthly plans |
| **June 1, 2026** | Existing enterprise PRU budgets auto-carry-over to AI Credits |
| **June 1, 2026** | Fallback to included models is **removed** |
| **June 1, 2026** | Budgets and user-level limits fully operational |
| **June 1, 2026** | Code review starts consuming GitHub Actions minutes |
| **June 1, 2026** | Model multipliers increase for annual individual subscribers |
| **June–Aug 2026** | **Promotional period** — elevated included credits for Business & Enterprise |
| **September 1, 2026** | Standard included credit amounts take effect |
| **Ongoing** | Annual individual subscribers migrate when their plan expires |

### Immediate Action Items (Now through May 31)

| Priority | Action | Owner |
|----------|--------|-------|
| 🔴 Critical | Preview projected bill using billing preview tool | Billing manager |
| 🔴 Critical | Download and analyze usage report (CSV) | Billing manager |
| 🔴 Critical | Decide overage policy (allow/block additional usage) | Enterprise owner |
| 🟠 High | Set up enterprise + org + user budgets | Enterprise/org owner |
| 🟠 High | Review and adjust existing PRU budgets (auto-carry-over) | Billing manager |
| 🟠 High | Communicate changes to all engineering teams | Engineering leadership |
| 🟡 Medium | Create model selection policy (which models, which teams) | Platform team |
| 🟡 Medium | Deploy `.copilotignore` and `copilot-instructions.md` in key repos | Engineering teams |
| 🟡 Medium | Run developer education workshop on token efficiency | Engineering leadership |
| 🟢 Low | Establish monthly review cadence for credit usage | Engineering managers |
| 🟢 Low | Create reusable prompt templates for common tasks | Team leads |

---

*Last updated: April 29, 2026*
