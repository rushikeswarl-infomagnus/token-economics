# Usage-Based Billing Scenarios — Copilot Enterprise

> **Plan:** Copilot Enterprise — $39/user/month  
> **Effective:** June 1, 2026  
> **Sources:** [Usage-Based Billing](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises), [Models and Pricing](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)

## Enterprise Billing at a Glance

| Item | Value |
|------|-------|
| Seat price | $39/user/month |
| Standard included usage | 3,900 AI Credits/user/month |
| Promotional usage (June–August 2026) | 7,000 AI Credits/user/month |
| Credit value | 1 AI Credit = $0.01 USD |
| Pooling | Included credits are pooled within the billing entity |
| Additional usage | Charged at model token rates when enabled |
| Code completions and Next Edit suggestions | Included; do not consume AI Credits |

Actual charges depend on the selected model and measured input, cache-read, and output tokens. The figures below are planning examples, not quotes.

## Scenario 1: 100-Seat Enterprise Within Its Pool

| Item | Credits | Cost |
|------|---------|------|
| Included pool (100 × 3,900) | 390,000 | Included in seats |
| Monthly consumption | 340,000 | Included |
| Remaining pool | 50,000 | — |
| Seat cost (100 × $39) | — | $3,900 |
| **Total monthly cost** | — | **$3,900** |

Unused credits from light users absorb heavier users' consumption through pooling.

## Scenario 2: 100-Seat Enterprise with Overage

| Item | Credits | Cost |
|------|---------|------|
| Included pool | 390,000 | Included in seats |
| Monthly consumption | 450,000 | — |
| Additional usage | 60,000 | $600 |
| Seat cost | — | $3,900 |
| **Total monthly cost** | — | **$4,500** |

This assumes additional usage is enabled. If it is blocked, AI-credit features stop when the applicable budget is exhausted; included code completions remain available.

## Scenario 3: Promotional Period

A 100-seat existing Enterprise customer receives a 700,000-credit promotional pool each month from June through August 2026.

| Item | Credits | Cost |
|------|---------|------|
| Promotional pool (100 × 7,000) | 700,000 | Included in seats |
| Monthly consumption | 450,000 | Included |
| Remaining pool | 250,000 | — |
| **Total monthly cost** | — | **$3,900** |

At the same usage after the promotion, the standard pool produces the $600 overage shown in Scenario 2.

## Scenario 4: Cost-Center Chargeback

For 200 seats, the standard included pool is 780,000 credits. Usage can be attributed to cost centers for internal reporting.

| Cost Center | Seats | Usage | Planning Share |
|-------------|------:|------:|---------------:|
| Product | 100 | 420,000 | 56% |
| Platform | 60 | 240,000 | 32% |
| Security | 40 | 90,000 | 12% |
| **Total** | **200** | **750,000** | **100%** |

The enterprise remains 30,000 credits below its shared pool, so total external cost is the $7,800 seat charge. A chargeback policy may allocate that cost by seats, usage, or a blended method.

## Scenario 5: Budget Guardrails

For the 100-seat example, administrators could:

1. Set alerts at 75%, 90%, and 100% of the planned pool.
2. Create cost-center and user budgets for accountability.
3. Decide whether usage continues as paid overage or stops at the configured limit.
4. Review model and token-type usage before changing limits.

Budgets are spending controls; they do not create additional included credits.

## Planning Checklist

- Confirm the seat count and standard or promotional allowance.
- Estimate usage by model, token type, organization, cost center, and user.
- Decide whether additional usage is allowed.
- Configure enterprise, organization, cost-center, and user budgets.
- Communicate that pooled credits are shared rather than individual allocations.
- Reforecast before the promotional allowance ends.
