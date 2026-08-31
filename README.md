# GitHub Copilot Token Economics Advisory

Advisory materials for navigating GitHub Copilot's transition from premium request (PRU) billing to **usage-based AI Credits billing**, effective **June 1, 2026**.

## Contents

| # | File | Description |
|---|------|-------------|
| 01 | [Main Advisory Brief](01-GitHub-Copilot-Token-Economics-Advisory.md) | Executive summary, AI Credits mechanics, model pricing tables, budget controls, FAQ (~20 Q&As) |
| 02 | [Token Economics Scenarios](02-Token-Economics-Scenarios.md) | 12 real-world scenarios — from a quick chat (0.07 credits) to a cloud agent session (117 credits) |
| 03 | [Best Practices for Token Management](03-Best-Practices-Token-Management.md) | 6 optimization levers, prompt engineering rules, context management, org controls, developer education |
| 04 | [Before vs After June 1 Charges](04-Before-vs-After-June1-Charges.md) | Side-by-side comparison of PRU billing vs AI Credits, 4 cost scenarios, data availability calendar |
| 05 | [AI Credits Business License Scenarios](05-AI-Credits-Business-License-Scenarios.md) | 20 detailed Business plan scenarios — individual interactions, daily profiles, team pooling, projections |
| 06 | [Delivery Implementation Playbook](06-Delivery-Implementation-Playbook.md) | 6-phase rollout plan: Triage → Baseline → Control → Optimize → Enable → Measure |
| 07 | [Enterprise UBB Billing Scenarios](07-UBB-Billing-Scenarios-Enterprise.md) ([HTML](07-UBB-Billing-Scenarios-Enterprise.html)) | Enterprise plan pooling, overage, promotion, chargeback, and budget scenarios |

## Key Facts

- **AI Credits**: 1 credit = $0.01 USD, pooled at the organization level
- **Business plan**: $19/user/month → 1,900 credits/user (promo: 3,000 until Aug 2026)
- **Enterprise plan**: $39/user/month → 3,900 credits/user (promo: 7,000 until Aug 2026)
- **Code completions**: Remain free and unlimited
- **Billing formula**: `(Input tokens × rate) + (Cache read tokens × rate) + (Output tokens × rate) = cost → credits`

## Audience

- **Engineering Leaders** — budget planning and team impact
- **FinOps / IT Admins** — budget hierarchy configuration and monitoring
- **Developers** — prompt optimization and model selection
- **Delivery / Operations** — rollout planning and change management

## License

Private — internal use only.
