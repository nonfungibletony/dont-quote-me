# 09 — Operations, Support & Compliance

## Human-in-the-Loop (HITL) System

### When HITL Triggers
- Job value >$5,000
- High complexity flag (agent confidence <70%)
- Unusual scope (specialist jobs, non-standard materials)
- Agent requests human review (rare, but possible)
- Customer complaint or escalation

### HITL Workflow
```
Agent flags for human review
   ↓
Job enters queue (Retool / custom dashboard)
   ↓
Human reviewer (estimator/trade expert) assesses within 10 minutes
   ↓
Reviewer can:
   - Approve as-is
   - Make minor edits (same chat thread, invisible to user)
   - Request more info from tradie (agent relays)
   ↓
Approved quote → back to agent → PDF generation → delivery
```

### Human Reviewer Profile
- Licensed tradesperson or experienced estimator
- AU-based (understands local codes, practices, suppliers)
- Available during business hours (7 AM – 7 PM AEST)
- Paid per review or hourly retainer
- KPI: <10 min response time, >95% accuracy approval rate

## Escalation Matrix

| Level | Trigger | Response | Timeframe |
|---|---|---|---|
| **L1 — Agent Self-Heal** | Minor pricing discrepancy, unclear photo | Agent asks clarifying question | Immediate |
| **L2 — Human Reviewer** | >$5k job, complex scope, low confidence | Estimator reviews | <10 minutes |
| **L3 — Team Lead** | Customer complaint, liability concern, data breach | Senior staff intervenes | <1 hour |
| **L4 — Founder/Legal** | Legal threat, regulatory issue, media inquiry | Executive + legal counsel | <4 hours |

## Customer Support Channels

| Channel | Purpose | Response Time |
|---|---|---|
| **In-Chat Support** | Quick questions, quote tweaks, "how do I…?" | <2 minutes (bot) / <10 minutes (human) |
| **Email** | Billing, account issues, feature requests | <24 hours |
| **Phone (optional)** | Urgent escalations, enterprise clients | <1 hour (business hours) |
| **WhatsApp/Telegram DM** | Direct line for high-tier users | <30 minutes |

## Liability & Disclaimers

### Standard Quote Disclaimer (Auto-Appended to Every PDF)
```
IMPORTANT: This quote is based on information provided by the customer and site inspection 
where available. Material prices are current as of [DATE/TIME] and are subject to change. 
This quote is valid for 30 days from issue. Final costs may vary based on site conditions, 
unforeseen issues, or changes to scope. Always verify quantities and prices before ordering. 
GST included. E&OE.
```

### Terms of Service (Key Clauses)
- **No guarantee of accuracy** — AI-generated quotes are drafts for review
- **Tradie responsibility** — Final quote approval and customer communication is tradie's responsibility
- **Price volatility** — Supplier prices change; agent uses best available data at time of generation
- **Privacy** — Job site photos are processed and auto-deleted; never used for model training without consent
- **Liability cap** — Maximum liability limited to value of the specific quote fee paid

### Insurance
- Professional indemnity insurance (recommended: $2M+ coverage)
- Cyber liability insurance (data breach protection)
- Public liability (if sending representatives to site)

## Data Privacy & Compliance

### Australian Privacy Principles (APP) Compliance
- **Collection:** Minimal — only what's needed to generate quote
- **Use:** Solely for quote generation and service improvement (with consent)
- **Storage:** AU data residency (AWS Sydney / Supabase AU region)
- **Retention:** Quote data retained for service improvement; photos auto-deleted after 24–48 hours
- **Access:** Tradie can request full export or deletion of their data anytime
- **Breach:** 72-hour notification protocol

### GDPR (If Expanding to UK/EU)
- Data Processing Agreement with all vendors
- Explicit consent for data usage
- Right to erasure (already supported)

### Security Measures
- End-to-end encryption for chat (WhatsApp native + Telegram MTProto)
- Row-level security in database (each tradie only sees own data)
- API keys stored in secure vault (not in code)
- Regular penetration testing (quarterly)
- SOC 2 Type II certification (target: Year 2)

## Compliance Checklist

| Requirement | Status | Owner | Deadline |
|---|---|---|---|
| AU Business Registration (ABN) | ☐ | Legal | Pre-launch |
| GST registration | ☐ | Legal | Pre-launch |
| Privacy Policy (APP compliant) | ☐ | Legal | Pre-launch |
| Terms of Service | ☐ | Legal | Pre-launch |
| Professional Indemnity Insurance | ☐ | Legal | Pre-launch |
| Cyber Liability Insurance | ☐ | Legal | Pre-launch |
| Data breach response plan | ☐ | Ops | Month 1 |
| GDPR readiness (if applicable) | ☐ | Legal | Month 6 |
| SOC 2 Type II audit | ☐ | Security | Year 2 |

## Operational KPIs

| Metric | Target | Measurement |
|---|---|---|
| Quote generation time (first draft) | <60 seconds | Automated tracking |
| Human review queue length | <5 jobs | Real-time dashboard |
| Human review response time | <10 minutes | Timer from flag to approval |
| Customer support response | <10 minutes (chat) | Zendesk/Intercom metrics |
| Support ticket resolution | <24 hours | Ticket system |
| Quote accuracy complaints | <2% of quotes | Post-quote survey |
| Data breach incidents | 0 | Security monitoring |
| Uptime | >99.5% | Infrastructure monitoring |

## Incident Response Playbook

### Scenario 1: Wrong Quote Sent to Customer
1. Immediate: Agent notifies tradie via chat
2. Tradie contacts customer with corrected quote
3. Root cause analysis: Was it agent error, input quality, or edge case?
4. If agent error: Refund quote fee + $100 goodwill credit
5. Update system prompt/rules to prevent recurrence

### Scenario 2: Data Breach
1. Immediate: Isolate affected systems
2. Within 1 hour: Assess scope (what data, how many users)
3. Within 24 hours: Notify affected users + OAIC (if required)
4. Within 72 hours: Public disclosure (if material)
5. Remediation: Patch vulnerability, audit all access logs

### Scenario 3: Supplier API Outage
1. Fallback to cached catalogue (last known prices)
2. Flag all quotes: "Bunnings pricing temporarily unavailable — using last known prices. Verify before ordering."
3. Notify users proactively
4. Escalate to supplier partnership team

---
