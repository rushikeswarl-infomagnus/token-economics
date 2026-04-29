# How AI Credits Are Charged — Copilot Business License Scenarios

> **Date:** April 29, 2026  
> **Plan:** Copilot Business — $19/user/month  
> **Effective:** June 1, 2026  
> **Source:** [GitHub Docs — Usage-Based Billing](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises), [Models & Pricing](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)

---

## How Copilot Business Billing Works After June 1

### The Basics

| Item | Value |
|------|-------|
| Seat price | **$19/user/month** (unchanged) |
| Included AI Credits per user | **1,900 credits** ($19 worth) |
| Promo credits (June–Aug 2026) | **3,000 credits/user** ($30 worth) |
| Credit value | **1 AI Credit = $0.01 USD** |
| Pooling | Credits are **pooled** across all users in the billing entity |
| Overage rate | Per-token rates if admin allows additional usage |
| Code completions | **Free** — not billed in AI Credits |
| Next Edit Suggestions | **Free** — not billed in AI Credits |

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

### Key Rules

- **Rates vary by model and token type** — each model has its own input, cache, and output rate
- **Tokens are not interchangeable** — input tokens, cache read tokens, and output tokens are priced separately
- **Code completions and Next Edit suggestions remain included** — they do not consume AI Credits
- **Copilot Code Review consumes AI Credits and GitHub Actions minutes** — dual billing

### How a Single Interaction Is Charged

```
Step 1: Developer sends a prompt (e.g., chat, agent, CLI)
Step 2: Copilot processes the request using the selected model
Step 3: Token consumption is calculated using the formula above:
        (Input tokens × input rate) + (Cache read tokens × cache rate) + (Output tokens × output rate)
Step 4: Dollar cost is converted to AI Credits (cost ÷ $0.01)
Step 5: Credits are deducted from the org's shared pool
```

### Token Estimation Quick Reference

| Item | Approximate Tokens |
|------|-------------------|
| 1 word | ~1.3 tokens |
| 1 line of code | ~15–25 tokens |
| 1 typical source file (200 lines) | ~3,000–5,000 tokens |
| A short chat prompt | ~200–500 tokens |
| A detailed prompt with context | ~2,000–8,000 tokens |
| A short response (explanation) | ~300–800 tokens |
| A full function generation | ~500–2,000 tokens |
| A multi-file agent output | ~10,000–50,000 tokens |

---

## Scenario 1: Simple Chat — "What Does This Error Mean?"

> Developer pastes a stack trace and asks Copilot to explain it.

| Component | Tokens | Model | Rate/1M | Cost |
|-----------|--------|-------|---------|------|
| Input (prompt + stack trace) | 600 | GPT-5 mini | $0.25 | $0.00015 |
| Output (explanation) | 400 | GPT-5 mini | $2.00 | $0.00080 |
| **Total** | **1,000** | | | **$0.00095** |

| | |
|--|--|
| **AI Credits consumed** | **~0.1 credits** |
| Monthly budget impact | 0.005% of 1,900 |
| How many of these fit in budget | **~19,000** |

---

## Scenario 2: Code Suggestion — "Write a Utility Function"

> "Write a TypeScript function that debounces an input handler with a configurable delay"

| Component | Tokens | Model | Rate/1M | Cost |
|-----------|--------|-------|---------|------|
| Input (prompt + system context) | 800 | GPT-4.1 | $2.00 | $0.0016 |
| Output (function + JSDoc) | 500 | GPT-4.1 | $8.00 | $0.0040 |
| **Total** | **1,300** | | | **$0.0056** |

| | |
|--|--|
| **AI Credits consumed** | **~0.56 credits** |
| Monthly budget impact | 0.03% of 1,900 |
| How many of these fit in budget | **~3,390** |

---

## Scenario 3: Code Explanation — "Explain This Complex Function"

> Developer selects a 150-line function and asks "Explain what this does step by step"

| Component | Tokens | Model | Rate/1M | Cost |
|-----------|--------|-------|---------|------|
| Input (prompt + 150 lines of code) | 3,000 | Claude Sonnet 4 | $3.00 | $0.009 |
| Output (detailed explanation) | 1,500 | Claude Sonnet 4 | $15.00 | $0.0225 |
| **Total** | **4,500** | | | **$0.0315** |

| | |
|--|--|
| **AI Credits consumed** | **~3.15 credits** |
| Monthly budget impact | 0.17% of 1,900 |
| How many of these fit in budget | **~603** |

---

## Scenario 4: Unit Test Generation

> "Generate comprehensive unit tests for this UserService class" (class is 200 lines)

| Component | Tokens | Model | Rate/1M | Cost |
|-----------|--------|-------|---------|------|
| Input (prompt + class code + imports) | 5,000 | GPT-5.2 | $1.75 | $0.00875 |
| Output (test file with 8 test cases) | 3,500 | GPT-5.2 | $14.00 | $0.049 |
| **Total** | **8,500** | | | **$0.058** |

| | |
|--|--|
| **AI Credits consumed** | **~5.8 credits** |
| Monthly budget impact | 0.3% of 1,900 |
| How many of these fit in budget | **~327** |

---

## Scenario 5: Bug Fix with Context — Agent Mode

> Agent mode: "Fix the race condition in our WebSocket handler" — Copilot reads 4 files, proposes changes to 2

| Component | Tokens | Model | Rate/1M | Cost |
|-----------|--------|-------|---------|------|
| Input (prompt + 4 files loaded) | 15,000 | Claude Sonnet 4.6 | $3.00 | $0.045 |
| Cached context (system prompt, prior turns) | 5,000 | Claude Sonnet 4.6 | $0.30 | $0.0015 |
| Output (analysis + 2 file patches) | 4,000 | Claude Sonnet 4.6 | $15.00 | $0.060 |
| **Total** | **24,000** | | | **$0.1065** |

| | |
|--|--|
| **AI Credits consumed** | **~10.7 credits** |
| Monthly budget impact | 0.56% of 1,900 |
| How many of these fit in budget | **~178** |

---

## Scenario 6: Multi-File Refactoring — Agent Session

> "Refactor all API route handlers to use the new error middleware" — touches 8 files

| Component | Tokens | Model | Rate/1M | Cost |
|-----------|--------|-------|---------|------|
| Input (prompt + 8 files loaded) | 40,000 | Claude Sonnet 4 | $3.00 | $0.120 |
| Cached context (across tool-call turns) | 20,000 | Claude Sonnet 4 | $0.30 | $0.006 |
| Cache write | 15,000 | Claude Sonnet 4 | $3.75 | $0.056 |
| Output (refactored code for 8 files) | 15,000 | Claude Sonnet 4 | $15.00 | $0.225 |
| **Total** | **90,000** | | | **$0.407** |

| | |
|--|--|
| **AI Credits consumed** | **~40.7 credits** |
| Monthly budget impact | 2.1% of 1,900 |
| How many of these fit in budget | **~47** |

---

## Scenario 7: Architecture Review with Frontier Model

> "Review our microservices architecture and identify coupling issues" — context from 15 files + architecture docs

| Component | Tokens | Model | Rate/1M | Cost |
|-----------|--------|-------|---------|------|
| Input (prompt + 15 files + docs) | 80,000 | Claude Opus 4.7 | $5.00 | $0.400 |
| Cached context | 20,000 | Claude Opus 4.7 | $0.50 | $0.010 |
| Cache write | 30,000 | Claude Opus 4.7 | $6.25 | $0.188 |
| Output (detailed analysis + recommendations) | 10,000 | Claude Opus 4.7 | $25.00 | $0.250 |
| **Total** | **140,000** | | | **$0.848** |

| | |
|--|--|
| **AI Credits consumed** | **~84.8 credits** |
| Monthly budget impact | 4.5% of 1,900 |
| How many of these fit in budget | **~22** |

---

## Scenario 8: Copilot Cloud Agent — Full Feature Implementation

> "Implement user registration with email verification, including model, controller, service, migration, and tests"

The cloud agent runs autonomously across multiple turns, reading and writing many files.

| Component | Tokens | Model | Rate/1M | Cost |
|-----------|--------|-------|---------|------|
| Input (cumulative across all turns) | 180,000 | GPT-5.2-Codex | $1.75 | $0.315 |
| Cached context (reused across turns) | 90,000 | GPT-5.2-Codex | $0.175 | $0.016 |
| Output (code + docs + tests across turns) | 60,000 | GPT-5.2-Codex | $14.00 | $0.840 |
| **Total** | **330,000** | | | **$1.171** |

| | |
|--|--|
| **AI Credits consumed** | **~117 credits** |
| Monthly budget impact | 6.2% of 1,900 |
| How many of these fit in budget | **~16** |

---

## Scenario 9: Copilot Code Review on a Pull Request

> PR with 400 changed lines across 6 files, assigned to Copilot for review

Code review has **dual billing**: AI Credits for model usage + GitHub Actions minutes for infrastructure.

| Component | Tokens/Minutes | Rate | Cost |
|-----------|---------------|------|------|
| Input (diff + file context) | 12,000 tokens | ~$2.00/1M (auto-selected model) | $0.024 |
| Output (review comments) | 2,500 tokens | ~$10.00/1M | $0.025 |
| **AI Credits subtotal** | | | **$0.049 (~4.9 credits)** |
| GitHub Actions minutes | ~4 minutes | $0.008/min (standard runner) | **$0.032** |
| **Combined total** | | | **$0.081** |

| | |
|--|--|
| **AI Credits consumed** | **~4.9 credits** (+ $0.032 Actions) |
| How many reviews fit in budget | **~388** (AI credits only) |

---

## Scenario 10: Copilot CLI — Terminal Commands

> "copilot explain: What does this kubectl command do?"

| Component | Tokens | Model | Rate/1M | Cost |
|-----------|--------|-------|---------|------|
| Input (command + prompt) | 300 | GPT-5 mini | $0.25 | $0.000075 |
| Output (explanation) | 500 | GPT-5 mini | $2.00 | $0.001 |
| **Total** | **800** | | | **$0.001** |

| | |
|--|--|
| **AI Credits consumed** | **~0.1 credits** |
| How many of these fit in budget | **~19,000** |

---

## Scenario 11: Spark — App Prototyping

> "Create a todo app with dark mode and local storage persistence"

Spark interactions tend to have larger outputs (full app generation).

| Component | Tokens | Model | Rate/1M | Cost |
|-----------|--------|-------|---------|------|
| Input (prompt + system context) | 2,000 | Varies | ~$2.00/1M | $0.004 |
| Output (full app code + styles) | 8,000 | Varies | ~$10.00/1M | $0.080 |
| **Total** | **10,000** | | | **$0.084** |

| | |
|--|--|
| **AI Credits consumed** | **~8.4 credits** |
| How many of these fit in budget | **~226** |

---

## Scenario 12: Repeated Conversations in a Day — Cumulative Costs

> Developer's typical workday: variety of interactions

| # | Activity | Model | Credits |
|---|----------|-------|---------|
| 1 | Morning standup prep — "Summarize my recent commits" | GPT-5 mini | 0.2 |
| 2 | Quick syntax question | GPT-5 mini | 0.1 |
| 3 | Generate a helper function | GPT-4.1 | 0.6 |
| 4 | Debug a failing test (with file context) | Claude Sonnet 4 | 5.0 |
| 5 | Write 3 unit tests | GPT-5.2 | 5.8 |
| 6 | Agent: Fix a bug across 2 files | Claude Sonnet 4.6 | 10.7 |
| 7 | Quick chat about API design | GPT-5 mini | 0.2 |
| 8 | Generate PR description | GPT-5 mini | 0.3 |
| 9 | Agent: Implement a small feature (3 files) | Claude Sonnet 4 | 25.0 |
| 10 | Quick question about Docker config | GPT-5 mini | 0.1 |
| 11 | Code review (Copilot auto-reviews a PR) | Auto-selected | 4.9 |
| 12 | Afternoon: Explain a colleague's complex code | Claude Sonnet 4 | 3.2 |
| 13 | Generate documentation for a module | GPT-5 mini | 1.0 |
| 14 | Agent: Refactor error handling (5 files) | Claude Sonnet 4 | 30.0 |
| 15 | End of day: Quick syntax lookup | GPT-5 mini | 0.1 |
| | **Daily Total** | | **87.2 credits** |

| | |
|--|--|
| **Daily credits** | ~87.2 |
| **Monthly (22 working days)** | **~1,918 credits** |
| **Business plan included** | 1,900 credits |
| **Overage** | **~18 credits ($0.18)** |

> ⚠️ This active developer barely exceeds the Business plan allowance. The agent sessions (#6, #9, #14) consume **75%** of the total.

---

## Scenario 13: Lightweight Developer — Chat Only, No Agent

> Developer who uses only chat, no agent mode or code review

| # | Activity | Model | Credits |
|---|----------|-------|---------|
| 1–20 | 20 quick chats per day | GPT-5 mini | 2.0 |
| 21–25 | 5 medium chats with code context | GPT-4.1 | 3.0 |
| | **Daily Total** | | **5.0 credits** |

| | |
|--|--|
| **Monthly (22 working days)** | **~110 credits** |
| **% of Business plan used** | **5.8%** |
| **Credits available for pooling** | **~1,790** |

> ✅ This developer uses almost nothing. Their unused 1,790 credits flow to the org pool for power users.

---

## Scenario 14: Power User — Agent-Heavy Workflow

> Senior engineer who relies heavily on agent mode for refactoring and feature work

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
| **Business plan included** | 1,900 credits |
| **Overage needed** | **2,621 credits ($26.21)** |

> ⚠️ This power user needs ~2.4x their individual share. Pooling from light users and the org pool is essential. In a team of 10, if 3 are light users contributing ~1,790 credits each, that's 5,370 extra credits to absorb this.

---

## Scenario 15: Team Budget — Pooling in Action

### Team Profile: 10 Business Users

| Role | Count | Monthly Credits/Person | Total Credits |
|------|-------|----------------------|---------------|
| Power users (agent-heavy) | 2 | 4,521 | 9,042 |
| Active developers (mixed) | 5 | 1,918 | 9,590 |
| Light users (chat only) | 3 | 110 | 330 |
| **Total Consumption** | **10** | | **18,962** |

### Without Pooling (Hypothetical)

| User Type | Included | Used | Overage | Overage Cost |
|-----------|----------|------|---------|-------------|
| 2 power users | 3,800 | 9,042 | 5,242 | $52.42 |
| 5 active devs | 9,500 | 9,590 | 90 | $0.90 |
| 3 light users | 5,700 | 330 | 0 | $0.00 |
| **Total** | **19,000** | **18,962** | **5,332** | **$53.32** |

### With Pooling (Actual New Model)

| | Credits |
|--|--------|
| Total included pool (10 × 1,900) | **19,000** |
| Total consumption | **18,962** |
| **Remaining in pool** | **38 credits** |
| **Overage cost** | **$0.00** |

> ✅ **Pooling saves this team $53.32/month.** The 3 light users' unused credits absorb the power users' overages entirely.

### Same Team During Promo Period (June–August)

| | Credits |
|--|--------|
| Total promo pool (10 × 3,000) | **30,000** |
| Total consumption | **18,962** |
| **Remaining in pool** | **11,038 credits** |
| **Headroom** | **37% buffer** |

---

## Scenario 16: Large Org — 100 Business Users

| Developer Profile | Count | Monthly Credits/Person | Total Credits |
|-------------------|-------|----------------------|---------------|
| Power users | 10 | 4,500 | 45,000 |
| Active developers | 50 | 1,900 | 95,000 |
| Moderate users | 25 | 800 | 20,000 |
| Light/occasional | 15 | 200 | 3,000 |
| **Total** | **100** | | **163,000** |

| | Credits | Cost |
|--|--------|------|
| Included pool (100 × 1,900) | 190,000 | $0 (included in $19/user) |
| Consumption | 163,000 | |
| **Remaining** | **27,000** | **$0 overage** |
| Seat cost | 100 × $19 | **$1,900/month** |
| **Total monthly cost** | | **$1,900/month** |

### If Consumption Spikes to 220,000 Credits

| | Credits | Cost |
|--|--------|------|
| Included pool | 190,000 | $0 |
| Overage | 30,000 | **$300** |
| Seat cost | | $1,900 |
| **Total monthly cost** | | **$2,200/month** |
| **Per-user effective cost** | | **$22/user/month** |

---

## Scenario 17: Model Selection Impact on Business Budget

> Same task performed with different models — "Review and refactor a 300-line function"  
> Input: ~6,000 tokens | Output: ~4,000 tokens

| Model | Input Cost | Output Cost | Total | Credits | % of 1,900 |
|-------|-----------|-------------|-------|---------|------------|
| GPT-5.4 nano | $0.0012 | $0.005 | $0.006 | **0.6** | 0.03% |
| GPT-5 mini | $0.0015 | $0.008 | $0.010 | **1.0** | 0.05% |
| Grok Code Fast 1 | $0.0012 | $0.006 | $0.007 | **0.7** | 0.04% |
| GPT-4.1 | $0.012 | $0.032 | $0.044 | **4.4** | 0.23% |
| Gemini 2.5 Pro | $0.0075 | $0.040 | $0.048 | **4.8** | 0.25% |
| Claude Haiku 4.5 | $0.006 | $0.020 | $0.026 | **2.6** | 0.14% |
| GPT-5.2 | $0.0105 | $0.056 | $0.067 | **6.7** | 0.35% |
| Claude Sonnet 4 | $0.018 | $0.060 | $0.078 | **7.8** | 0.41% |
| Claude Opus 4.7 | $0.030 | $0.100 | $0.130 | **13.0** | 0.68% |
| GPT-5.5 | $0.030 | $0.120 | $0.150 | **15.0** | 0.79% |

**The same task costs 0.6 to 15 credits — a 25x difference based solely on model choice.**

### Budget Stretch: How Many of This Task Fits in 1,900 Credits

| Model | Tasks/Month | Category |
|-------|-------------|----------|
| GPT-5.4 nano | ~3,167 | Best value |
| GPT-5 mini | ~1,900 | Good value |
| GPT-4.1 | ~432 | Moderate |
| Claude Sonnet 4 | ~244 | Expensive |
| Claude Opus 4.7 | ~146 | Very expensive |
| GPT-5.5 | ~127 | Most expensive |

---

## Scenario 18: What Happens When Budget Is Exhausted

### Configuration A: Additional Usage Allowed (No User Budget)

```
Day 1–18:  Normal usage, pool depleted gradually
Day 19:    Pool exhausted (1,900 × N credits used)
Day 19–30: Usage continues, billed at per-token rates
Day 30:    Invoice includes: seat cost + overage charges
```

**Example:** 10-user team, pool = 19,000 credits, consumed 25,000 credits
- Overage: 6,000 credits × $0.01 = **$60 additional charge**
- Total: $190 (seats) + $60 (overage) = **$250/month**

### Configuration B: Additional Usage Blocked

```
Day 1–18:  Normal usage, pool depleted gradually
Day 19:    Pool exhausted
Day 19–30: ALL USERS BLOCKED from using AI-credit features
           (Code completions still work — they're free)
Day 30:    Pool resets to included amount
```

> ⚠️ No fallback to cheaper models. Developers lose access to Chat, Agent, CLI, Code Review, etc.

### Configuration C: User-Level Budget Set ($25/user)

```
Power user:
  Day 1–12:  Uses 2,500 credits ($25 budget)
  Day 12:    USER BLOCKED — personal budget exhausted
  Day 12–30: Cannot use Copilot AI features even if org pool has credits
  
Light user:
  Day 1–30:  Uses 110 credits (~$1.10)
  Status:    Never hits budget, works normally all month
```

> ⚠️ User-level budgets override pool availability. A blocked user cannot access Copilot even if the team pool has thousands of credits remaining.

### Configuration D: Enterprise Budget with Alerts (Recommended)

```
Enterprise budget: $3,000/month for 100 users
Alert thresholds: 75% ($2,250), 90% ($2,700), 100% ($3,000)

Day 10:  75% consumed → Email alert to billing manager
Day 13:  90% consumed → Urgent alert, consider reviewing usage
Day 15:  100% consumed → Hard stop OR allow overage (admin choice)
```

---

## Scenario 19: Comparing Old Overage vs. New Overage

> Team of 20 Business users, 500 interactions above the included allowance

### Old Model (Before June 1)

| Item | Calculation | Cost |
|------|------------|------|
| Included | 20 × 300 = 6,000 PRUs | $0 |
| 500 extra interactions | 500 × $0.04 | **$20.00** |
| Seat cost | 20 × $19 | $380.00 |
| **Total** | | **$400.00** |

### New Model (After June 1) — If Those 500 Interactions Are Short Chats

| Item | Calculation | Cost |
|------|------------|------|
| Included pool | 20 × 1,900 = 38,000 credits | $0 |
| 500 short chats (GPT-5 mini, ~0.1 credits each) | 50 credits × $0.01 | **$0.50** |
| Seat cost | 20 × $19 | $380.00 |
| **Total** | | **$380.50** |

### New Model — If Those 500 Interactions Are Agent Sessions

| Item | Calculation | Cost |
|------|------------|------|
| Included pool | 38,000 credits | $0 |
| 500 agent sessions (~40 credits each) | 20,000 credits overage × $0.01 | **$200.00** |
| Seat cost | 20 × $19 | $380.00 |
| **Total** | | **$580.00** |

> **Key insight:** Under the old model, all 500 interactions cost $20 regardless of complexity. Under the new model, the same "500 extra interactions" could cost anywhere from **$0.50 to $200+** depending on what those interactions actually are.

---

## Scenario 20: Month-Over-Month Cost Projection

### 50-User Business Org — First 6 Months

| Month | Pool | Consumption | Overage | Seat Cost | Total |
|-------|------|-------------|---------|-----------|-------|
| **June** (promo) | 150,000 | 80,000 | $0 | $950 | **$950** |
| **July** (promo) | 150,000 | 95,000 | $0 | $950 | **$950** |
| **August** (promo) | 150,000 | 110,000 | $0 | $950 | **$950** |
| **September** | 95,000 | 115,000 | $200 | $950 | **$1,150** |
| **October** | 95,000 | 105,000 | $100 | $950 | **$1,050** |
| **November** | 95,000 | 90,000 | $0 | $950 | **$950** |

**Pattern:** Usage typically increases as developers adopt agent workflows. The promo period (June–Aug) provides a buffer while teams learn to optimize. By November, optimization efforts and model selection policies reduce consumption.

**6-month total: $6,000** (vs. $5,700 at pure seat cost = ~5% effective increase)

---

## Quick Reference: Credits-per-Activity for Business Budgeting

| Activity | Cheap Model | Mid Model | Premium Model |
|----------|------------|-----------|---------------|
| Quick chat question | 0.05–0.1 | 0.5–1.0 | 2–5 |
| Code generation (single function) | 0.3–0.7 | 2–5 | 8–15 |
| Code explanation | 0.2–0.5 | 3–5 | 10–15 |
| Unit test generation | 0.5–1.5 | 5–8 | 15–25 |
| Bug fix (single file) | 0.3–1.0 | 3–8 | 10–20 |
| Agent: multi-file edit (3–5 files) | N/A | 20–40 | 60–100 |
| Agent: large refactor (8+ files) | N/A | 40–80 | 100–200 |
| Cloud agent (full feature) | N/A | 80–150 | 200–400 |
| Architecture review | N/A | 30–60 | 80–150 |
| Code review (per PR) | N/A | 3–8 | 8–15 |
| CLI command | 0.05–0.1 | 0.3–0.5 | 1–3 |
| Spark prototype | N/A | 5–10 | 15–30 |

### Business Plan Monthly Capacity by Usage Pattern

| If a developer only does... | Credits/Day | Credits/Month | Fits in 1,900? |
|----------------------------|------------|---------------|----------------|
| 50 quick chats (GPT-5 mini) | 5 | 110 | ✅ 6% used |
| 20 code generations (GPT-4.1) | 11 | 242 | ✅ 13% used |
| 10 code explanations (Sonnet 4) | 32 | 704 | ✅ 37% used |
| 3 agent sessions (Sonnet 4) | 75 | 1,650 | ✅ 87% used |
| 5 agent sessions (Sonnet 4) | 125 | 2,750 | ❌ 145% — needs pool |
| 2 cloud agent sessions (GPT-5.2-Codex) | 234 | 5,148 | ❌ 271% — needs pool |
| 1 architecture review (Opus 4.7) | 85 | 1,870 | ⚠️ 98% — tight |

---

*Last updated: April 29, 2026*
