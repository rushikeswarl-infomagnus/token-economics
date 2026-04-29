# Delivery Implementation Playbook — Usage-Based Billing

> **Date:** April 29, 2026  
> **Audience:** Admin + FinOps + Engineering Operations  
> **Delivery Time:** 10–15 min per phase  
> **Effective:** June 1, 2026

---

## Playbook Overview

Run this as a joint effort across **Admin**, **FinOps**, and **Engineering Operations** to prepare your organization for the transition to usage-based billing.

```
  ① ──────→ ② ──────→ ③ ──────→ ④ ──────→ ⑤ ──────→ ⑥
Triage     Baseline   Control   Optimize   Enable    Measure
```

| Phase | Name | Focus |
|-------|------|-------|
| **1** | **Triage** | Plan, seats, current budgets, preview data |
| **2** | **Baseline** | Users, models, features, included vs. additional |
| **3** | **Control** | Enterprise, cost center, user, SKU budgets |
| **4** | **Optimize** | Auto mode, context, prompts, tools, agents |
| **5** | **Enable** | Admin/FinOps briefing + developer micro-training |
| **6** | **Measure** | Weekly transition report + escalation path |

### Output Package

At the end of this playbook, you will have:

- ✅ **Owner map** — who owns what across billing, governance, and developer enablement
- ✅ **Budget design** — enterprise → org → cost center → user budget hierarchy
- ✅ **Configured controls** — overage policies, model access, user-level limits
- ✅ **Prompt/token guidance** — team-specific optimization patterns
- ✅ **Reporting template** — weekly dashboard for the transition period
- ✅ **Optimization backlog** — prioritized list of cost reduction opportunities

---

## Phase 1: Triage

> **Goal:** Understand your current position and identify risk areas.

### What to Do

| # | Action | Owner | Output |
|---|--------|-------|--------|
| 1.1 | Confirm your Copilot plan (Business or Enterprise) | Admin | Plan type documented |
| 1.2 | Count total assigned seats | Admin | Seat count |
| 1.3 | Review current premium request budgets | FinOps | Existing budget limits |
| 1.4 | Use the **billing preview tool** (enterprise home page banner) | FinOps | Side-by-side current vs. projected cost |
| 1.5 | Download the **preview data CSV** | FinOps | CSV with `aic_quantity` and `aic_gross_amount` |
| 1.6 | Identify high-impact accounts (top 10 consumers) | FinOps | Risk user list |

### Key Questions to Answer

- [ ] How many seats do we have and on which plan?
- [ ] What is our current monthly premium request spend?
- [ ] What does the billing preview show as our projected AI Credit cost?
- [ ] Who are our top 10 consumers by premium request volume?
- [ ] Are there any accounts that would significantly exceed their credit share?

### Triage Decision Matrix

| Preview Result | Risk Level | Immediate Action |
|---------------|------------|-----------------|
| Projected spend **< included credits** | 🟢 Low | Proceed to Phase 2, minimal urgency |
| Projected spend **≈ included credits** (within 10%) | 🟡 Medium | Proceed to Phase 2, plan budgets carefully |
| Projected spend **> included credits** by 10–30% | 🟠 High | Prioritize Phase 3 (controls), identify top consumers |
| Projected spend **> included credits** by 30%+ | 🔴 Critical | Immediate executive briefing, fast-track all phases |

---

## Phase 2: Baseline

> **Goal:** Build a detailed picture of how your organization uses Copilot today.

### What to Do

| # | Action | Owner | Output |
|---|--------|-------|--------|
| 2.1 | Analyze usage by **user** — who uses the most? | FinOps | User consumption ranking |
| 2.2 | Analyze usage by **model** — which models consume the most? | FinOps | Model distribution chart |
| 2.3 | Analyze usage by **feature** — Chat vs. Agent vs. Cloud Agent vs. Code Review | FinOps | Feature breakdown |
| 2.4 | Calculate **included vs. additional** — how much fits in the pool vs. overage? | FinOps | Included/overage split |
| 2.5 | Map users to teams/cost centers | Admin | User → team mapping |
| 2.6 | Identify which models are "included" (GPT-4.1, GPT-5 mini) vs. premium | Engineering Ops | Model tier classification |

### Baseline Report Template

```
Organization: _______________
Plan: Business / Enterprise
Total Seats: ___
Billing Period Analyzed: April 2026

USAGE SUMMARY
─────────────
Total Premium Requests (current model):  ___
Projected AI Credits (new model):        ___
Projected Cost (new model):              $___
Included Credit Pool (seats × rate):     ___
Projected Overage:                       ___ credits ($___) 

TOP 5 USERS BY CONSUMPTION
───────────────────────────
1. _____________ — ___ credits (___% of pool)
2. _____________ — ___ credits (___% of pool)
3. _____________ — ___ credits (___% of pool)
4. _____________ — ___ credits (___% of pool)
5. _____________ — ___ credits (___% of pool)

MODEL DISTRIBUTION
──────────────────
GPT-5 mini / GPT-4.1 (included): ___% of interactions
Sonnet / Gemini (mid-tier):       ___% of interactions
Opus / GPT-5.5 (frontier):        ___% of interactions

FEATURE DISTRIBUTION
────────────────────
Chat:         ___% of credits
Agent mode:   ___% of credits
Cloud Agent:  ___% of credits
Code Review:  ___% of credits
CLI:          ___% of credits
Other:        ___% of credits
```

### Key Insight to Extract

**What percentage of total credits are consumed by agent/cloud agent sessions?**

In most organizations, agent workflows consume 60–90% of total credits despite being <20% of interactions. This is the #1 optimization target.

---

## Phase 3: Control

> **Goal:** Configure budget guardrails before June 1 so costs don't surprise you.

### Budget Hierarchy Design

```
Enterprise Budget ($X/month)
  ├── Org A Budget ($Y/month)
  │     ├── Cost Center: Platform Team ($Z/month)
  │     ├── Cost Center: Product Team ($Z/month)
  │     └── Cost Center: Data Team ($Z/month)
  │           ├── User Budget: Senior Eng ($U/month or uncapped)
  │           ├── User Budget: Mid Eng ($U/month)
  │           └── User Budget: Contractor ($U/month, capped)
  └── Org B Budget ($Y/month)
        └── ...
```

### What to Do

| # | Action | Owner | Output |
|---|--------|-------|--------|
| 3.1 | Set **enterprise-level budget** with alert thresholds (75%, 90%, 100%) | FinOps / Admin | Enterprise budget configured |
| 3.2 | Set **org-level budgets** for each organization | Admin | Org budgets configured |
| 3.3 | Create **cost centers** mapped to teams or business units | FinOps | Cost centers created |
| 3.4 | Set **cost-center budgets** | FinOps | Per-team budgets |
| 3.5 | Decide on **user-level budgets** (if needed) | Admin / FinOps | User budget policy |
| 3.6 | Configure **overage policy** — allow or block additional usage | Admin | Policy set |
| 3.7 | Configure **SKU-level budgets** (Copilot, Spark, Cloud Agent separately) | FinOps | SKU budgets set |
| 3.8 | Set **model access policy** — which models are available to which teams | Admin | Model governance |

### Budget Sizing Guide

| Scenario | Enterprise Budget | User-Level Budget | Overage Policy |
|----------|------------------|-------------------|---------------|
| **Conservative** | Included pool only | $20–25/user | Block at limit |
| **Balanced** | Included + 10–15% buffer | $30–40/user (engineers), $10 (non-eng) | Allow with alerts |
| **Growth-oriented** | Included + 25–30% buffer | Uncapped for engineers, capped for others | Allow, review monthly |

### Overage Policy Decision Tree

```
Do you want to allow usage beyond the included credit pool?
  │
  ├── YES → Set an enterprise budget cap (e.g., 120% of included)
  │           ├── Configure alert at 75%, 90%, 100%
  │           └── Review monthly, adjust quarterly
  │
  └── NO → Hard stop at pool exhaustion
              ├── ⚠️ Developers LOSE access (no fallback models)
              ├── Code completions still work (free)
              └── Risk: productivity loss in last week of month
```

### Control Checklist

- [ ] Enterprise budget set with alert thresholds
- [ ] Org-level budgets configured
- [ ] Cost centers created and mapped to teams
- [ ] User-level budgets applied (if using)
- [ ] Overage policy configured (allow/block)
- [ ] Model access policies reviewed
- [ ] Existing PRU budgets reviewed (auto-carry-over on June 1)

---

## Phase 4: Optimize

> **Goal:** Reduce token waste without suppressing high-value adoption.

### The 6 Optimization Levers

| Lever | Action | Expected Impact |
|-------|--------|----------------|
| **Auto mode** | Enable auto model selection as default; route by task intent, avoid defaulting every task to frontier models | 🟢 10–20% credit reduction |
| **Context hygiene** | Compact, summarize, or start a new chat when context diverges; deploy `.copilotignore` and `copilot-instructions.md` | 🟢 10–30% input token reduction |
| **Prompt scoping** | Ask for constrained, staged outputs instead of one giant request; use prompt templates | 🟡 5–15% output token reduction |
| **Agent discipline** | Use agents for high-value workflows only; avoid fleets of agents for trivial work | 🟠 20–50% credit reduction on agent spend |
| **Tool / MCP hygiene** | Enable tools only when needed; broad tool context can increase usage | 🟡 5–10% input token reduction |
| **User limits** | Cap aggressive consumption while preserving pilot exceptions | 🟢 Prevents outlier spend |

### What to Do

| # | Action | Owner | Output |
|---|--------|-------|--------|
| 4.1 | Enable **auto model selection** as org default in VS Code | Engineering Ops | Model routing configured |
| 4.2 | Deploy **`.copilotignore`** in top repositories (exclude build artifacts, vendor, lock files) | Engineering Ops | Ignore files committed |
| 4.3 | Deploy **`.github/copilot-instructions.md`** in top repositories (persistent context) | Engineering Ops | Instructions committed |
| 4.4 | Create **prompt templates** (`.prompt.md`) for common tasks (bug fix, test gen, code review) | Team Leads | Templates created and shared |
| 4.5 | Define **agent usage guidelines** — which tasks justify agent mode vs. chat | Engineering Ops | Agent policy documented |
| 4.6 | Audit **enabled MCP servers/tools** — disable unused ones | Engineering Ops | Tool list cleaned up |
| 4.7 | Create **architecture summaries** for key repos (reduces context scanning) | Team Leads | ARCHITECTURE.md files created |

### Optimization Backlog Template

| Priority | Optimization | Owner | Est. Savings | Status |
|----------|-------------|-------|-------------|--------|
| 🔴 P0 | Enable auto model selection org-wide | Eng Ops | 10–20% | ☐ Not started |
| 🔴 P0 | Deploy .copilotignore in top 10 repos | Eng Ops | 10–15% input | ☐ Not started |
| 🟠 P1 | Agent discipline guidelines | Eng Ops | 20–50% agent | ☐ Not started |
| 🟠 P1 | Prompt templates for top 5 use cases | Team Leads | 5–15% output | ☐ Not started |
| 🟡 P2 | copilot-instructions.md in top 10 repos | Eng Ops | 5–10% input | ☐ Not started |
| 🟡 P2 | MCP/tool audit and cleanup | Eng Ops | 5–10% input | ☐ Not started |
| 🟢 P3 | Architecture summaries for monorepos | Team Leads | Variable | ☐ Not started |

---

## Phase 5: Enable

> **Goal:** Educate admins, FinOps, and developers so everyone understands the change.

### Two-Track Enablement

#### Track A: Admin / FinOps Briefing (30 min)

| Time | Topic | Content |
|------|-------|---------|
| 0–10 min | What's changing | AI Credits overview, pooling, no-fallback, code review dual billing |
| 10–20 min | Budget controls | 4-level budget hierarchy, overage policies, alert configuration |
| 20–25 min | Monitoring tools | Billing overview, premium request analytics, usage reports, REST API |
| 25–30 min | Transition timeline | Data availability calendar, office hours, promo period |

#### Track B: Developer Micro-Training (15 min)

| Time | Topic | Content |
|------|-------|---------|
| 0–3 min | What's changing for you | Credits replace PRUs, included models now cost tokens, no fallback |
| 3–7 min | Model selection | When to use cheap vs. premium models (cheat sheet) |
| 7–11 min | Token efficiency | 5 quick wins: be specific, constrain output, one task per prompt, use @file, plan first |
| 11–13 min | Agent discipline | Agent sessions = 50–150 credits; chat = 0.1–5 credits; choose wisely |
| 13–15 min | How to check your usage | VS Code status bar → Copilot icon; start new chats when context bloats |

### Communication Templates

#### Email to Engineering Teams

```
Subject: GitHub Copilot billing is changing on June 1 — what you need to know

Hi team,

Starting June 1, Copilot moves from premium requests to usage-based billing 
with AI Credits. Here's what changes for you:

✅ UNCHANGED: Code completions and Next Edit Suggestions remain free and unlimited.
✅ UNCHANGED: Your Copilot access is not going away.

🔄 CHANGING: AI interactions (chat, agent, CLI, code review) will now be 
measured by tokens consumed, not flat requests.

⚡ KEY ACTIONS:
1. Pick the right model for the job (GPT-5 mini for quick Qs, Sonnet for complex tasks)
2. Be specific in your prompts — vague prompts waste tokens on bad outputs
3. Use agent mode for high-value tasks, not quick questions
4. Check your usage: VS Code → Copilot icon in status bar

We've set up budgets and controls so you don't need to worry about surprise costs.
If you have questions, reach out to [contact].

Full guide: [link to best practices doc]
```

#### Slack Announcement

```
📢 *Copilot billing change — June 1*

TL;DR: Premium requests → AI Credits (token-based). Your access is unchanged.

*3 things to know:*
1. Model choice matters more than ever — GPT-5 mini is cheap, Opus is 25x more
2. Agent sessions use 50–150 credits; chat uses 0.1–5. Choose wisely.
3. Code completions remain free and unlimited.

Quick wins: be specific, scope prompts, plan before generating.
Full guide: [link]
```

### Enable Checklist

- [ ] Admin/FinOps briefing scheduled and delivered
- [ ] Developer micro-training scheduled (per team or org-wide)
- [ ] Model selection cheat sheet distributed
- [ ] Email/Slack announcement sent
- [ ] FAQ shared (link to advisory doc)
- [ ] Office hours scheduled for Q&A (May 6/7 window)

---

## Phase 6: Measure

> **Goal:** Track the transition weekly, surface issues early, and iterate.

### Weekly Transition Report Template

```
WEEK OF: _______________
REPORT #: ___

CREDIT CONSUMPTION
──────────────────
Total pool available:          ___ credits
Credits consumed this week:    ___ credits
Month-to-date consumption:     ___ credits
Projected end-of-month:        ___ credits
Pool remaining:                ___ credits (___%)

BUDGET STATUS
─────────────
Enterprise budget:   ___% consumed │ Alert triggered: Yes / No
Org budgets:         Any at >80%? ___
User budgets:        Any exhausted? ___

TOP CONSUMERS THIS WEEK
───────────────────────
1. _________ — ___ credits (model: ___, feature: ___)
2. _________ — ___ credits
3. _________ — ___ credits

MODEL MIX
─────────
Included (GPT-4.1, GPT-5 mini):  ___% of interactions
Mid-tier (Sonnet, Gemini):        ___% of interactions
Frontier (Opus, GPT-5.5):         ___% of interactions

AGENT USAGE
───────────
Agent sessions this week:         ___
Avg credits per agent session:    ___
Agent share of total credits:     ___%

ISSUES / ESCALATIONS
────────────────────
• ___
• ___

ACTIONS FOR NEXT WEEK
─────────────────────
• ___
• ___
```

### Escalation Path

| Signal | Severity | Action | Owner |
|--------|----------|--------|-------|
| Pool >75% consumed by mid-month | 🟡 Watch | Review top consumers, check for waste | FinOps |
| Pool >90% consumed by mid-month | 🟠 Alert | Notify engineering leads, consider model guidance | FinOps + Eng Ops |
| Single user consuming >10% of pool | 🟠 Alert | 1:1 review — is this high-value or waste? | Eng Manager |
| Pool exhausted before month-end | 🔴 Critical | Executive decision: allow overage or block | Admin + FinOps |
| User blocked by individual budget | 🟡 Watch | Evaluate if budget increase is justified | Eng Manager |
| Agent sessions >70% of total spend | 🟠 Alert | Review agent discipline — are agents justified? | Eng Ops |
| Frontier models >30% of interactions | 🟡 Watch | Remind teams of model selection guidance | Eng Ops |

### Measure Cadence

| Timeframe | Activity | Owner |
|-----------|----------|-------|
| **Daily** (June) | Quick check: any budget alerts triggered? | FinOps |
| **Weekly** (June–Aug) | Full transition report using template above | FinOps + Eng Ops |
| **Bi-weekly** (Sep onward) | Usage review, budget adjustment | FinOps |
| **Monthly** | Executive summary: cost, adoption, optimization progress | FinOps + Admin |
| **Quarterly** | Policy review: budgets, model access, user limits | Admin + Eng Leadership |

---

## Owner Map

| Area | Primary Owner | Backup | Responsibilities |
|------|--------------|--------|-----------------|
| **Billing & Budgets** | FinOps / Billing Manager | Enterprise Owner | Budget setup, alerts, overage policy, usage reports |
| **Model & Feature Governance** | Engineering Operations | Platform Team | Model access policies, auto mode config, tool audits |
| **Developer Enablement** | Engineering Leadership | Team Leads | Training, communication, prompt templates, agent guidelines |
| **Cost Center Management** | FinOps | Admin | Cost center creation, team mapping, SKU budgets |
| **Escalation Handling** | FinOps + Eng Ops | Admin | Weekly report, escalation triage, budget adjustments |
| **Executive Reporting** | Admin / FinOps | Engineering Leadership | Monthly summary, quarterly policy review |

---

## Full Output Package Checklist

At the end of executing this playbook, confirm you have:

- [ ] **Owner map** — documented with names, roles, and responsibilities
- [ ] **Budget design** — enterprise → org → cost center → user hierarchy configured
- [ ] **Configured controls** — overage policies set, model governance applied, alerts active
- [ ] **Prompt/token guidance** — optimization levers communicated, templates distributed
- [ ] **Reporting template** — weekly transition report ready to run
- [ ] **Optimization backlog** — prioritized list of actions with owners and timelines

---

*Last updated: April 29, 2026*
