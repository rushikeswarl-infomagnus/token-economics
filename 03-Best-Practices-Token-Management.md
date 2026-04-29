# Best Practices for Token Management

> **Date:** April 29, 2026  
> **Audience:** Engineering Teams, Team Leads, Platform/DevOps Teams, Engineering Managers

---

## Table of Contents

0. [What Gets Counted](#0-what-gets-counted)
1. [Model Selection Strategy](#1-model-selection-strategy)
2. [Prompt Engineering for Efficiency](#2-prompt-engineering-for-efficiency)
3. [Context Management](#3-context-management)
4. [Workflow Optimization](#4-workflow-optimization)
5. [Organizational Controls](#5-organizational-controls)
6. [Developer Education](#6-developer-education)
7. [Monitoring & Continuous Improvement](#7-monitoring--continuous-improvement)

---

## 0. What Gets Counted

Every optimization starts with understanding what you're paying for. Three types of tokens are measured, rated, and billed:

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

**Why this matters for best practices:**
- **Input tokens** — you control these directly. Smaller, more targeted context = fewer input tokens = lower cost.
- **Output tokens** — the most expensive component. Constraining output format ("return only the function") saves the most money.
- **Cache read tokens** — the cheapest. Keeping conversation context stable maximizes cache hits and lowers cost.

---

## 1. Model Selection Strategy

### The #1 Lever: Choosing the Right Model

Model selection has the **single biggest impact** on token costs. The same task can cost 1x with a lightweight model or 23x with a frontier model.

### Model Selection Cheat Sheet

| Task Type | Recommended Model(s) | Cost Tier | Why |
|-----------|----------------------|-----------|-----|
| Syntax questions, quick lookups | GPT-5 mini, GPT-5.4 nano | 💚 Lowest | Simple recall, no reasoning needed |
| Documentation generation | GPT-5 mini, Grok Code Fast 1 | 💚 Lowest | Formulaic output, low complexity |
| Commit messages, PR descriptions | GPT-5 mini | 💚 Lowest | Short output, pattern-based |
| Boilerplate/CRUD generation | GPT-4.1, GPT-5 mini | 💚 Low | Well-understood patterns |
| Unit test scaffolding | GPT-4.1, Raptor mini | 💚 Low | Repetitive structure |
| Standard code generation | GPT-4.1, Claude Sonnet 4 | 💛 Medium | Needs code understanding |
| Code explanation & review | Claude Sonnet 4, Gemini 2.5 Pro | 💛 Medium | Requires comprehension |
| Bug debugging (single file) | Claude Sonnet 4, GPT-5.2 | 💛 Medium | Needs reasoning |
| Multi-file refactoring | Claude Sonnet 4.6, GPT-5.2-Codex | 🟠 High | Complex, cross-file understanding |
| Architecture design | Claude Opus 4.6/4.7, GPT-5.5 | 🔴 Highest | Deep reasoning, system-level thinking |
| Security audit | Claude Opus 4.6, GPT-5.4 | 🔴 Highest | Requires thorough analysis |
| Novel algorithm design | GPT-5.5, Claude Opus 4.7 | 🔴 Highest | Frontier reasoning capability |

### Model Routing Rules for Teams

1. **Default to included/lightweight models** for everyday tasks (GPT-5 mini, GPT-4.1)
2. **Upgrade to versatile models** (Sonnet 4, GPT-5.2) for tasks requiring real reasoning
3. **Reserve frontier models** (Opus, GPT-5.5) for high-value, high-complexity work
4. **Use auto model selection** in VS Code when unsure — it was discounted under PRU model
5. **Never use frontier models for documentation** — that's burning $25/M-output tokens on work a $2/M model handles

---

## The 6 Optimization Levers

> **Reduce waste first. Do not suppress high-value adoption.**

```
┌──────────────────────────────────────┐  ┌──────────────────────────────────────┐
│  Auto mode                           │  │  Context hygiene                     │
│                                      │  │                                      │
│  Route by task intent; avoid         │  │  Compact, summarize, or start a new  │
│  defaulting every task to            │  │  chat when context diverges.         │
│  frontier models.                    │  │                                      │
└──────────────────────────────────────┘  └──────────────────────────────────────┘
┌──────────────────────────────────────┐  ┌──────────────────────────────────────┐
│  Prompt scoping                      │  │  Agent discipline                    │
│                                      │  │                                      │
│  Ask for constrained, staged outputs │  │  Use agents for high-value           │
│  instead of one giant request.       │  │  workflows; avoid fleets of agents   │
│                                      │  │  for trivial work.                   │
└──────────────────────────────────────┘  └──────────────────────────────────────┘
┌──────────────────────────────────────┐  ┌──────────────────────────────────────┐
│  Tool / MCP hygiene                  │  │  User limits                         │
│                                      │  │                                      │
│  Enable tools only when needed;      │  │  Cap aggressive consumption while    │
│  broad tool context can increase     │  │  preserving pilot exceptions.        │
│  usage.                              │  │                                      │
└──────────────────────────────────────┘  └──────────────────────────────────────┘
```

### Lever 1: Auto Mode
- Let Copilot route by task intent — don't manually select frontier models for every interaction
- Lightweight questions get lightweight models; complex reasoning gets premium models
- **Action:** Enable auto model selection as the team default

### Lever 2: Context Hygiene
- Long conversations accumulate context tokens that are re-sent every turn
- **Compact or summarize** when conversations go beyond 10–15 turns
- **Start a new chat** when the topic diverges from the original task
- **Action:** Train developers to recognize when context is bloated and reset

### Lever 3: Prompt Scoping
- Ask for constrained, staged outputs instead of one giant request
- "Write the function signature and docstring" → review → "Now implement the body"
- **Action:** Establish team patterns for staged prompting

### Lever 4: Agent Discipline
- Use agents for high-value workflows (multi-file refactors, feature implementation)
- **Avoid fleets of agents for trivial work** — a quick chat is 0.1 credits; an agent session is 50+
- **Action:** Define which tasks justify agent mode vs. simple chat

### Lever 5: Tool / MCP Hygiene
- Enable tools only when needed — each enabled tool adds context tokens to every request
- Broad tool context can significantly increase per-interaction costs
- **Action:** Audit enabled MCP servers and tools; disable those not actively needed

### Lever 6: User Limits
- Cap aggressive consumption while preserving pilot exceptions
- Set user-level budgets for roles with limited AI needs
- Allow generous or uncapped budgets for high-value engineering roles
- **Action:** Implement tiered user budgets aligned to role and usage patterns

---

## 2. Prompt Engineering for Efficiency

### The 5 Rules of Token-Efficient Prompting

#### Rule 1: Be Specific in the First Sentence
```
❌ "Fix the bug in our app"
   → Forces model to search/guess, produces generic response, likely needs retry

✅ "In src/auth/oauth.ts, the token refresh at line 45 fails when concurrent 
    requests arrive. Fix it using a mutex pattern."
   → Precise, scoped, likely produces correct result on first attempt
```

#### Rule 2: State the Output Format
```
❌ "Write a validation function"
   → May return full file, imports, tests, explanation you didn't ask for

✅ "Write a single validateEmail(email: string): boolean function. 
    Return only the function body, no imports or tests."
   → Constrains output tokens to only what you need
```

#### Rule 3: One Task Per Prompt
```
❌ "Refactor the auth module, add logging, write tests, and update the README"
   → Compound task = longer output, higher chance of errors, expensive retry

✅ Prompt 1: "Refactor the auth module to use dependency injection"
   Prompt 2: "Add structured logging to the refactored auth module"
   Prompt 3: "Write unit tests for the auth module"
   → Each task is cheaper, more accurate, and individually verifiable
```

#### Rule 4: Provide Constraints
```
❌ "Build a REST API client"
   → Model may choose any library, language features, patterns

✅ "Build a REST API client using only the Node.js standard library (no axios, 
    no node-fetch). Use async/await. Target Node 20+."
   → Reduces exploration in the model's response, shorter output
```

#### Rule 5: Don't Repeat Yourself
```
❌ Pasting the same 200-line file in every message of a conversation
   → Each paste = fresh input tokens at full price

✅ Reference with @file or rely on cached context
   → Cached tokens cost ~10x less than fresh input
```

### Prompt Templates

Create reusable `.prompt.md` files for your team's common tasks:

**Bug Fix Template:**
```markdown
File: {{file_path}}
Error: {{error_message}}
Expected behavior: {{expected}}
Actual behavior: {{actual}}
Constraints: Fix only the affected function. Do not modify the API signature.
Output: Only the changed function(s).
```

**Code Review Template:**
```markdown
Review the following diff for:
1. Logic errors
2. Security vulnerabilities
3. Performance issues
Respond with a numbered list of findings. Skip style/formatting feedback.
```

**Test Generation Template:**
```markdown
File: {{file_path}}
Function: {{function_name}}
Generate unit tests covering:
- Happy path
- Edge cases (null, empty, boundary values)
- Error conditions
Framework: {{test_framework}}
Output: Only test code, no explanations.
```

---

## 3. Context Management

### Minimize Input Token Waste

Input tokens are cheaper than output tokens, but at scale they add up — especially in large codebases.

#### Use `.copilotignore` to Exclude Noise
Create a `.copilotignore` file in your repo root:
```
# Build artifacts
dist/
build/
out/

# Dependencies
node_modules/
vendor/

# Generated code
*.generated.ts
*.pb.go
*_pb2.py

# Large data files
*.csv
*.json (data fixtures)
*.sql (dumps)

# Lock files
package-lock.json
yarn.lock
pnpm-lock.yaml
```

#### Close Irrelevant Files
Many IDE integrations send **open file context** to the model. If you have 30 tabs open, you're sending 30 files of context tokens. Close files you're not actively working on.

#### Use Targeted References
```
❌ @workspace "find the database connection code"
   → Scans entire workspace, loads many files into context

✅ @file src/db/connection.ts "explain the connection pool settings"
   → Loads one specific file
```

#### Maintain Architecture Summaries
A 200-line `ARCHITECTURE.md` is cheaper than the AI re-discovering your architecture from 50 source files every session:

```markdown
# Architecture Summary
## Services
- auth-service: OAuth2/JWT authentication (src/auth/)
- payment-service: Stripe integration (src/payments/)
- notification-service: Email/SMS via SNS (src/notifications/)

## Key Patterns
- Repository pattern for data access
- Event-driven communication via SQS
- Middleware chain for request processing

## Database
- PostgreSQL 15 via Prisma ORM
- Redis for session caching
```

#### Use `copilot-instructions.md`
Place a `.github/copilot-instructions.md` in your repo with persistent context:
```markdown
This is a TypeScript monorepo using pnpm workspaces.
Node 20+, strict TypeScript, ESM modules.
Testing: Vitest. Linting: ESLint flat config.
API style: RESTful with Zod validation.
Auth: JWT with refresh token rotation.
```
This loads once and is cached — far cheaper than explaining your stack in every prompt.

---

## 4. Workflow Optimization

### Plan First, Generate Second

**The spec-first approach saves the most tokens:**

1. **Plan with a cheap model** (GPT-5 mini, ~0.07 credits):  
   "Outline the steps to implement user registration with email verification"

2. **Review the plan** (human checkpoint — 0 credits):  
   Approve, modify, or redirect before expensive generation

3. **Execute with an appropriate model** (Sonnet 4, ~8 credits):  
   "Implement step 1 from the plan: create the User model with Prisma schema"

4. **Iterate per step** — not "do everything at once"

**Cost comparison:**
| Approach | Estimated Credits |
|----------|------------------|
| "Build the whole feature" → bad result → retry | ~120+ credits |
| Plan → approve → execute step-by-step | ~40–60 credits |

### Chunk Large Tasks

Instead of one massive prompt:
```
❌ "Migrate our entire API from Express to Fastify"
```

Break it into incremental steps:
```
✅ Step 1: "List all Express-specific patterns in src/routes/ that need changes"
   Step 2: "Convert src/routes/users.ts from Express to Fastify"
   Step 3: "Convert src/routes/payments.ts from Express to Fastify"
   Step 4: "Update the middleware chain for Fastify"
   Step 5: "Write integration tests for the migrated routes"
```

Each step:
- Costs less individually
- Is verifiable before proceeding
- Avoids expensive "redo everything" if one step fails

### Leverage Cached Tokens

Cached tokens cost **~10x less** than fresh input. To maximize cache hits:
- Keep system prompts and context stable across turns in a conversation
- Don't rephrase the same context in different words (breaks cache matching)
- Use conversation continuity — don't start new chat sessions for related tasks
- Reference files via `@file` rather than pasting content

### Human-in-the-Loop Checkpoints

```
Prompt 1: "Propose 3 approaches for implementing rate limiting" (cheap, planning)
    ↓ Human reviews and picks approach B
Prompt 2: "Implement approach B using Redis sliding window" (targeted, efficient)
    ↓ Human reviews code
Prompt 3: "Add error handling for Redis connection failures" (incremental)
```

vs.

```
Prompt 1: "Implement rate limiting with error handling and tests" (expensive, may miss expectations)
    ↓ Human doesn't like approach
Prompt 2: "Actually, use Redis sliding window instead" (full regeneration, waste)
    ↓ Human reviews, finds missing error handling
Prompt 3: "Add error handling too" (third pass, triple cost)
```

### Use Summarization Instead of Raw Files

When referencing previous work:
```
❌ Pasting 500 lines of code from a previous session

✅ "The auth module (summarized): handles JWT issuance/validation, 
    uses refresh token rotation, stores tokens in Redis with 24h TTL. 
    Now add rate limiting to the /login endpoint."
```

---

## 5. Organizational Controls

### Budget Architecture

**Recommended 4-tier setup:**

```
Enterprise Budget: $X/month (alert at 80%, hard stop at 100%)
  └── Org Budget: $Y/month per org
       └── Cost Center Budgets: By team/BU
            └── User Budgets: $Z/month (optional, for specific roles)
```

**Budget sizing guidelines:**

| Role | Suggested User Budget | Rationale |
|------|----------------------|-----------|
| Senior/Staff Engineers | No individual cap | Rely on org pool; high ROI users |
| Mid-level Engineers | No cap or generous cap | Productive users, learning curve |
| Junior Engineers | Moderate cap ($25–40) | Guide toward efficient usage |
| Non-engineering (PMs, designers) | Low cap ($5–10) | Limited AI interaction needs |
| Contractors | Explicit cap | Cost containment |

### Model Governance Policy

**Tier 1 — Unrestricted (all developers):**
- GPT-5 mini, GPT-4.1, GPT-5.4 nano, Grok Code Fast 1, Raptor mini

**Tier 2 — Available (all developers, monitored):**
- Claude Sonnet 4/4.5/4.6, GPT-5.2, Gemini 2.5 Pro, Claude Haiku 4.5

**Tier 3 — Restricted (approval or specific teams only):**
- Claude Opus 4.5/4.6/4.7, GPT-5.5, GPT-5.4

### Overage Policy Decision

| Policy | When to Use | Risk |
|--------|-------------|------|
| **Allow overages** | Innovation-focused teams, time-sensitive projects | Unpredictable costs |
| **Hard stop at limit** | Strict budget environments, non-critical use | Developer frustration, blocked work |
| **Allow with user budget cap** | Balanced approach — let org pool flex, cap individuals | Requires active monitoring |

**Recommended:** Allow overages at org level with user-level soft caps + alerts. Review monthly.

---

## 6. Developer Education

### What to Communicate to Teams

**Key messages:**
1. "We're not limiting your AI usage — we're making it visible and sustainable"
2. "Model choice matters more than prompt count — pick the right tool for the job"
3. "The same task can cost 1x or 23x depending on model choice"
4. "Agent sessions consume 90%+ of credits — scope them carefully"
5. "Well-crafted prompts save money AND produce better results"

### Quick Reference Card for Developers

```
┌───────────────────────────────────────────────────┐
│         TOKEN-EFFICIENT COPILOT USAGE             │
├───────────────────────────────────────────────────┤
│                                                   │
│  🎯 MODEL SELECTION                               │
│  • Quick questions → GPT-5 mini (cheapest)       │
│  • Standard coding → GPT-4.1 (included)          │
│  • Complex tasks → Sonnet 4 (balanced)           │
│  • Architecture → Opus/GPT-5.5 (expensive)       │
│                                                   │
│  ✍️  PROMPT TIPS                                   │
│  • Be specific in sentence 1                     │
│  • State desired output format                   │
│  • One task per prompt                           │
│  • Provide constraints                           │
│  • Don't repeat context — use @file              │
│                                                   │
│  ⚡ WORKFLOW                                       │
│  • Plan first (cheap model), execute second      │
│  • Chunk large tasks into steps                  │
│  • Review before regenerating                    │
│  • Close irrelevant tabs                         │
│  • Use .copilotignore for large repos            │
│                                                   │
│  📊 MONITOR                                       │
│  • Check usage: VS Code status bar → Copilot     │
│  • Credits reset on the 1st of each month        │
│                                                   │
└───────────────────────────────────────────────────┘
```

### Workshop Agenda (90 minutes)

| Time | Topic | Activity |
|------|-------|----------|
| 0–15 min | What's changing and why | Walkthrough of billing changes |
| 15–30 min | Model selection | Live demo: same task, 3 different models, show cost difference |
| 30–50 min | Prompt optimization | Hands-on: rewrite 5 bad prompts into efficient ones |
| 50–65 min | Workflow patterns | Demo: plan-first approach vs. single-shot |
| 65–80 min | Context management | Setup: `.copilotignore`, `copilot-instructions.md`, architecture docs |
| 80–90 min | Q&A and team-specific policies | Open discussion |

---

## 7. Monitoring & Continuous Improvement

### Weekly Review Checklist

- [ ] Check org-level credit consumption vs. budget
- [ ] Identify top 5 users by credit consumption
- [ ] Review model distribution — are expensive models being used for simple tasks?
- [ ] Check regeneration patterns — which users are retrying frequently?
- [ ] Verify budget alerts are configured and received

### Monthly Optimization Cycle

1. **Download usage report** (CSV with `aic_quantity` and `aic_gross_amount`)
2. **Analyze by model** — what % of spend goes to each model?
3. **Analyze by user** — who are the power users? Are they productive?
4. **Analyze by feature** — Chat vs. Agent vs. Cloud Agent vs. Code Review
5. **Identify optimization opportunities:**
   - Users consistently using Opus for simple tasks
   - High regeneration rates (poor prompt quality)
   - Agent sessions with excessive context loading
6. **Update policies** — adjust budgets, model access, team guidelines
7. **Share wins** — "Team X reduced credits 40% while increasing PR throughput"

### Key Metrics to Track

| Metric | Formula | Target |
|--------|---------|--------|
| Credit Efficiency | Credits consumed / PRs merged | Declining trend |
| Model Optimization Rate | % of prompts using cost-appropriate model | >80% |
| Regeneration Rate | Retry prompts / total prompts | <15% |
| Pool Utilization | Credits used / credits available | 70–90% |
| Overage Rate | Overage credits / included credits | <10% |
| Cost per Developer | Total credits / active developers | Within budget |

---

*Last updated: April 29, 2026*
