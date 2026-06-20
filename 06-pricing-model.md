# 06 — Pricing Model & Unit Economics

## Pricing Philosophy

Pay for the outcome, not the tool. The tradie should feel they're hiring a super-fast virtual employee, not subscribing to software.

## Pricing Tiers

### Tier 1: Pay-Per-Quote (Starter / Casual)
| Volume | Price/Quote | Best For |
|---|---|---|
| 1–10 quotes/month | $29 | Solo tradies testing the service |
| 11–50 quotes/month | $19 | Regular users |
| 51+ quotes/month | $14 | High-volume tradies |

**Features:**
- Full quote generation (BOM, pricing, PDF)
- Basic RAG memory
- 3D sketch preview
- Standard support

### Tier 2: Monthly Unlimited (Power User)
| Plan | Price/Month | Best For |
|---|---|---|
| Unlimited Standard | $149 | Regular tradies doing 10+ quotes/month |
| Unlimited Pro | $249 | Small teams, white-label branding |

**Features (Unlimited Standard):**
- Unlimited quotes
- Full RAG memory + monthly insights report
- Branded PDFs with logo
- Priority support

**Features (Unlimited Pro):**
- Everything in Standard
- White-label: "Quote Assistant for [Your Company]"
- Team sharing (up to 5 users)
- Xero/MYOB export
- Human review included for all jobs

### Tier 3: Enterprise / White-Label
| Package | Price | Best For |
|---|---|---|
| Business Bundle | $499+/month | Larger trade businesses, franchises |

**Features:**
- Everything in Pro
- Custom branding
- Dedicated account manager
- API access for integrations
- Volume discounts
- Custom trade rule configuration

## Onboarding Incentives

- **First 3 quotes free** (or $9 each for pay-per-quote tier)
- **First month 50% off** Unlimited plans
- **Referral program:** Both tradie and referee get 5 free quotes
- **Quote credits never expire** and roll over — creates sunk-cost feel

## Unit Economics

### Cost per Quote (at scale)

| Component | Cost |
|---|---|
| LLM API (Claude/GPT-4o, ~3 calls/quote) | $0.15–$0.40 |
| WhatsApp API (per message) | $0.005–$0.02 |
| Storage + DB (per quote) | $0.01 |
| PDF generation | $0.01 |
| 3D sketch (image gen, optional) | $0.05–$0.15 |
| Human review (if triggered, ~15% of quotes) | $2–$5 |
| **Total variable cost** | **$0.20–$1.00** |

### Revenue per Quote

| Tier | Price | Gross Margin |
|---|---|---|
| Pay-per-quote ($29) | $29 | ~97% |
| Pay-per-quote ($19) | $19 | ~95% |
| Unlimited ($149/mo, ~20 quotes) | $7.45 effective | ~90% |

### Customer Lifetime Value (CLV)

| Metric | Conservative | Optimistic |
|---|---|---|
| Average quotes/month | 15 | 25 |
| Average price/quote | $22 | $18 (volume discount) |
| Monthly revenue | $330 | $450 |
| Average retention | 8 months | 14 months |
| **CLV** | **$2,640** | **$6,300** |

### Payback Period (Tradie ROI)

**Scenario: Solo electrician, 15 quotes/month, $29/quote**

| Item | Value |
|---|---|
| Time saved per quote | 2.5 hours |
| Hourly billable rate | $85 |
| Value of time saved | $212.50/quote |
| Cost of service | $29/quote |
| **Net value per quote** | **$183.50** |
| **ROI per quote** | **633%** |
| Payback period | Immediate (first quote) |

**Scenario: Small team, Unlimited Pro ($249/month), 30 quotes/month**

| Item | Value |
|---|---|
| Time saved per quote | 2.5 hours |
| Hourly billable rate | $85 |
| Value of time saved | $6,375/month |
| Cost of service | $249/month |
| **Net value per month** | **$6,126** |
| **ROI per month** | **2,460%** |
| Payback period | Immediate |

## Comparison to Alternatives

| Solution | Cost per Quote | Time |
|---|---|---|
| Tradie doing it themselves | $0 (but 2–3 hours lost billable time = $170–$255) | 2–3 hours |
| Outsourced estimator | $100–$500 | 24–48 hours |
| AI takeoff tool (Togal, etc.) | $149–$299/month subscription | 30–60 minutes |
| **Don't Quote Me** | **$14–$29** | **Minutes** |

## Pricing Page Copy (Web)

```
Stop paying with your time.

Most quoting tools still make you do all the work.
Don't Quote Me is different — you text us the job details, 
we return a finished quote.

No app to learn. No subscription if you only quote 5 jobs a month.
Just pay for what you use.

[Pay-Per-Quote: $29] — [Unlimited: $149/mo] — [Pro: $249/mo]

First 3 quotes heavily discounted. No contracts. Cancel anytime.
```

## Financial Projections (Year 1–3)

| Metric | Year 1 | Year 2 | Year 3 |
|---|---|---|---|
| Active tradies | 200 | 1,000 | 3,500 |
| Avg quotes/month/tradie | 12 | 15 | 18 |
| Total quotes/month | 2,400 | 15,000 | 63,000 |
| Avg revenue/quote | $24 | $21 | $18 |
| Monthly revenue | $57,600 | $315,000 | $1,134,000 |
| Annual revenue | $691,200 | $3,780,000 | $13,608,000 |
| Gross margin | ~95% | ~96% | ~97% |
| Fixed costs (team, infra) | $240,000 | $600,000 | $1,500,000 |
| **Net profit** | **~$416,000** | **~$3,000,000** | **~$11,000,000** |

---
