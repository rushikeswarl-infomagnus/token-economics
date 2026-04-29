# Token Economics — Real-World Scenarios

> **Reference:** All prices per 1M tokens. 1 AI Credit = $0.01 USD.  
> **Date:** April 29, 2026

---

## How to Read These Scenarios

Each scenario estimates token consumption for a typical interaction. Actual costs vary based on prompt length, context window, and response size. These are representative estimates to help teams plan budgets.

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

**Token estimation rules of thumb:**
- 1 token ≈ 4 characters ≈ ¾ of a word
- 1 line of code ≈ 15–25 tokens
- 1 page of text ≈ 500 tokens
- A typical source file (200 lines) ≈ 3,000–5,000 tokens

---

## Scenario 1: Quick Chat Question (Low Cost)

> **"What does the `async` keyword do in Python?"**

| Component | Tokens | Model | Rate | Cost |
|-----------|--------|-------|------|------|
| Input (prompt + system) | ~500 | GPT-5 mini | $0.25/1M | $0.000125 |
| Output (answer) | ~300 | GPT-5 mini | $2.00/1M | $0.000600 |
| **Total** | **~800** | | | **$0.000725** |
| **AI Credits** | | | | **~0.07 credits** |

**Takeaway:** Simple Q&A with a lightweight model costs virtually nothing. A developer could do **~27,000 of these per month** within a Business plan's 1,900 credits.

---

## Scenario 2: Code Generation — Simple Function (Low Cost)

> **"Write a Python function to validate email addresses using regex"**

| Component | Tokens | Model | Rate | Cost |
|-----------|--------|-------|------|------|
| Input (prompt + context) | ~800 | GPT-4.1 | $2.00/1M | $0.0016 |
| Output (function + tests) | ~600 | GPT-4.1 | $8.00/1M | $0.0048 |
| **Total** | **~1,400** | | | **$0.0064** |
| **AI Credits** | | | | **~0.64 credits** |

**Takeaway:** ~2,970 simple code generation tasks per month within Business plan.

---

## Scenario 3: Code Generation — Complex Module (Medium Cost)

> **"Generate an OAuth2 middleware with token refresh, error handling, and retry logic for our Express.js app"**

| Component | Tokens | Model | Rate | Cost |
|-----------|--------|-------|------|------|
| Input (prompt + 3 context files) | ~8,000 | Claude Sonnet 4 | $3.00/1M | $0.024 |
| Output (module + types + tests) | ~4,000 | Claude Sonnet 4 | $15.00/1M | $0.060 |
| **Total** | **~12,000** | | | **$0.084** |
| **AI Credits** | | | | **~8.4 credits** |

**Takeaway:** ~226 complex module generations per month within Business plan.

---

## Scenario 4: Agent Mode — Multi-File Refactoring (High Cost)

> **Agent session: "Refactor our authentication layer to use the new JWT library across all 12 service files"**

Agent mode involves multiple tool calls and iterations. Only user prompts count as requests, but all tokens (tool calls, file reads, code generation) consume credits.

| Component | Tokens | Model | Rate | Cost |
|-----------|--------|-------|------|------|
| Input (prompt + 12 files context) | ~50,000 | Claude Sonnet 4.6 | $3.00/1M | $0.150 |
| Cached context (across iterations) | ~30,000 | Claude Sonnet 4.6 | $0.30/1M | $0.009 |
| Output (refactored code, 12 files) | ~25,000 | Claude Sonnet 4.6 | $15.00/1M | $0.375 |
| **Total** | **~105,000** | | | **$0.534** |
| **AI Credits** | | | | **~53.4 credits** |

**Takeaway:** ~35 such sessions per month within Business plan. These are the sessions to watch.

---

## Scenario 5: Architecture Planning with Frontier Model (Very High Cost)

> **"Analyze our microservices architecture and propose a migration plan from monolith to event-driven design"**

| Component | Tokens | Model | Rate | Cost |
|-----------|--------|-------|------|------|
| Input (prompt + architecture docs + 20 files) | ~80,000 | Claude Opus 4.7 | $5.00/1M | $0.400 |
| Output (detailed migration plan) | ~15,000 | Claude Opus 4.7 | $25.00/1M | $0.375 |
| **Total** | **~95,000** | | | **$0.775** |
| **AI Credits** | | | | **~77.5 credits** |

**Takeaway:** Only ~24 such sessions per month within Business plan. Reserve frontier models for high-value tasks.

---

## Scenario 6: Copilot Cloud Agent — Long Autonomous Session (Very High Cost)

> **"Implement the complete user registration flow including database models, API endpoints, validation, tests, and documentation"**

A cloud agent session runs autonomously, potentially for hours.

| Component | Tokens | Model | Rate | Cost |
|-----------|--------|-------|------|------|
| Input (cumulative across turns) | ~200,000 | GPT-5.2-Codex | $1.75/1M | $0.350 |
| Cached tokens | ~100,000 | GPT-5.2-Codex | $0.175/1M | $0.018 |
| Output (code + docs + tests) | ~80,000 | GPT-5.2-Codex | $14.00/1M | $1.120 |
| **Total** | **~380,000** | | | **$1.488** |
| **AI Credits** | | | | **~148.8 credits** |

**Takeaway:** Only ~12 of these sessions per month within Business plan. Scope agent tasks carefully.

---

## Scenario 7: Copilot Code Review on Pull Request (Medium Cost + Actions Minutes)

> **PR with 500 lines changed across 8 files, assigned to Copilot for review**

| Component | Tokens | Model | Rate | Cost |
|-----------|--------|-------|------|------|
| Input (diff + file context) | ~15,000 | Auto-selected | ~$2.00/1M | $0.030 |
| Output (review comments) | ~3,000 | Auto-selected | ~$10.00/1M | $0.030 |
| **AI Credits Subtotal** | **~18,000** | | | **$0.060 (~6 credits)** |
| GitHub Actions Minutes | ~5 min | Standard runner | $0.008/min | $0.040 |
| **Combined Total** | | | | **$0.100** |

**Takeaway:** Code review has a dual cost component. ~190 reviews per month within Business plan (AI credits only), but Actions minutes add up separately.

---

## Scenario 8: Daily Developer Usage — Typical Mixed Workload

> **One developer's typical day: 30 chat questions, 5 code generations, 2 agent sessions, 1 code review**

| Activity | Count | Credits Each | Total Credits |
|----------|-------|-------------|---------------|
| Quick chat (GPT-5 mini) | 30 | ~0.07 | 2.1 |
| Code generation (GPT-4.1) | 5 | ~0.64 | 3.2 |
| Agent sessions (Sonnet 4) | 2 | ~53.4 | 106.8 |
| Code review | 1 | ~6.0 | 6.0 |
| **Daily Total** | | | **~118 credits** |
| **Monthly (22 working days)** | | | **~2,596 credits** |

**Analysis:** This developer would slightly exceed the Business plan's 1,900 included credits. The agent sessions consume **90% of the budget**. Solutions:
1. Use pooled credits from lighter users
2. Reduce agent sessions or use cheaper models for them
3. Set user-level budget with allowance for overage

---

## Scenario 9: Enterprise Team of 50 — Monthly Projection

| Developer Profile | Count | Monthly Credits/Person | Total Credits |
|-------------------|-------|----------------------|---------------|
| Heavy (agent-heavy workflow) | 10 | ~3,000 | 30,000 |
| Moderate (chat + code gen) | 25 | ~1,200 | 30,000 |
| Light (occasional chat) | 15 | ~400 | 6,000 |
| **Total Consumption** | **50** | | **66,000** |

| | Credits |
|--|--------|
| Included Pool (50 × 1,900) | 95,000 |
| Projected Consumption | 66,000 |
| **Remaining** | **29,000 (30% buffer)** |

**During Promo Period (June–Aug):**

| | Credits |
|--|--------|
| Included Pool (50 × 3,000) | 150,000 |
| Projected Consumption | 66,000 |
| **Remaining** | **84,000 (56% buffer)** |

**Takeaway:** Pooling makes the Business plan workable for most teams. The 3-month promo gives generous headroom to establish baselines.

---

## Scenario 10: Model Choice Impact — Same Task, Different Models

> **Task: "Explain and refactor a 200-line sorting algorithm"**  
> Input: ~5,000 tokens, Output: ~3,000 tokens

| Model | Input Cost | Output Cost | Total | AI Credits | Relative Cost |
|-------|-----------|-------------|-------|------------|---------------|
| GPT-5.4 nano | $0.001 | $0.00375 | $0.005 | 0.5 | **1x (baseline)** |
| GPT-5 mini | $0.00125 | $0.006 | $0.007 | 0.7 | 1.4x |
| GPT-4.1 | $0.010 | $0.024 | $0.034 | 3.4 | **6.8x** |
| Claude Sonnet 4 | $0.015 | $0.045 | $0.060 | 6.0 | **12x** |
| Claude Opus 4.7 | $0.025 | $0.075 | $0.100 | 10.0 | **20x** |
| GPT-5.5 | $0.025 | $0.090 | $0.115 | 11.5 | **23x** |

**Takeaway:** The same task can cost **23x more** with a frontier model vs. a lightweight one. Model selection is the #1 lever for cost control.

---

## Scenario 11: Prompt Waste — Regeneration Penalty

> **Developer sends a vague prompt, gets a bad result, regenerates 3 times before success**

| Attempt | Input Tokens | Output Tokens | Model | Cost |
|---------|-------------|---------------|-------|------|
| 1st (vague prompt) | 2,000 | 3,000 | Claude Sonnet 4 | $0.051 |
| 2nd (slightly better) | 2,500 | 3,500 | Claude Sonnet 4 | $0.060 |
| 3rd (more specific) | 3,000 | 2,500 | Claude Sonnet 4 | $0.047 |
| **Wasted Total** | | | | **$0.111 (waste)** |
| | | | | |
| If done right 1st time | 3,000 | 2,500 | Claude Sonnet 4 | **$0.047** |

**Takeaway:** Poor prompting caused **2.4x the cost** of a well-crafted prompt. At scale across 100 developers, this waste becomes significant.

---

## Scenario 12: Large Monorepo Context — Scoped vs. Unscoped

> **Task: Fix a bug in the payment module of a 500-file monorepo**

### Unscoped (Bad Practice)
| Component | Tokens | Cost |
|-----------|--------|------|
| Input (repo-wide context loaded) | 150,000 | $0.450 (Sonnet 4) |
| Output | 2,000 | $0.030 |
| **Total** | **152,000** | **$0.480 (~48 credits)** |

### Scoped (Best Practice)
| Component | Tokens | Cost |
|-----------|--------|------|
| Input (only payment module files) | 15,000 | $0.045 (Sonnet 4) |
| Output | 2,000 | $0.030 |
| **Total** | **17,000** | **$0.075 (~7.5 credits)** |

**Takeaway:** Scoping context reduced cost by **6.4x**. In large codebases, this is the highest-impact optimization.

---

## Summary: Cost Ranges by Activity Type

| Activity Type | Credits per Interaction | Monthly Budget Impact |
|--------------|----------------------|----------------------|
| Quick chat (lightweight model) | 0.05–0.15 | Negligible |
| Simple code generation | 0.5–2.0 | Low |
| Complex code generation | 5–15 | Moderate |
| Agent session (standard model) | 30–80 | **High** |
| Agent session (frontier model) | 80–200 | **Very High** |
| Cloud agent (long session) | 100–300 | **Very High** |
| Architecture analysis (frontier) | 50–100 | **High** |
| Code review (per PR) | 5–15 + Actions min | Moderate |

---

*Last updated: April 29, 2026*
