# 12 — Risks & Mitigations

## Risk Matrix

| Risk | Likelihood | Impact | Overall | Mitigation | Owner |
|---|---|---|---|---|---|
| **Accuracy failures on quotes** | Medium | **Critical** | 🔴 High | Smart defaults + flagged assumptions; human review for >$5k; continuous learning | Product |
| **Low tradie adoption** | Medium | High | 🟡 Medium | WhatsApp/Telegram native (zero app); first 3 quotes free; referral incentives | Growth |
| **Competitor feature creep** | Medium | High | 🟡 Medium | Narrow focus on chat-native experience; personal RAG flywheel; speed to market | Strategy |
| **Supplier API instability** | Medium | Medium | 🟡 Medium | Cached catalogue fallback; multi-supplier strategy; SLA agreements | Engineering |
| **Liability / legal challenge** | Low | **Critical** | 🟡 Medium | Strong disclaimers; professional indemnity insurance; liability cap in ToS | Legal |
| **Data breach / privacy violation** | Low | **Critical** | 🟡 Medium | AU data residency; auto-delete media; row-level security; encryption; SOC 2 target | Security |
| **LLM cost inflation** | Medium | Medium | 🟡 Medium | Multi-model fallback; cost monitoring (LangSmith + Helicone); optimisation | Engineering |
| **WhatsApp/Telegram API changes** | Low | High | 🟢 Low | Maintain both channels; abstraction layer; direct API relationships | Engineering |
| **Human reviewer availability** | Medium | Medium | 🟡 Medium | Multiple reviewers on retainer; async queue; escalation to senior | Ops |
| **Price volatility (materials)** | High | Medium | 🟡 Medium | Real-time disclaimers; provisional sums; cached + live pricing hybrid | Product |
| **Scaling infra failures** | Low | High | 🟢 Low | Serverless architecture; auto-scaling; monitoring + alerting; redundant regions | Engineering |
| **Regulatory changes (GST, consumer law)** | Low | Medium | 🟢 Low | Legal monitoring; flexible pricing/tax engine; compliance advisory | Legal |

## Detailed Risk Analysis

### Risk 1: Quote Accuracy Failures
**Description:** AI hallucinates quantities, misses scope, or uses wrong materials. Tradie sends bad quote to customer → loses job or eats loss.

**Triggers:**
- Blurry or low-quality photos
- Unclear voice notes with background noise
- Unusual job scope not in training data
- Rapid material spec changes

**Mitigation Strategy:**
1. **Smart defaults + clear assumption flagging** — never hide what the agent assumed
2. **Confidence scoring** — if <70%, auto-flag for human review
3. **Human review gate** — all >$5k jobs reviewed by licensed estimator
4. **Tradie confirmation step** — "Review this before sending" on every quote
5. **Continuous feedback loop** — tradie reports errors → agent learns

**Contingency:**
- If accuracy drops below 80% for a trade, pause new signups for that trade until fixed
- Offer full refund + $100 credit for any quote that causes a lost job due to agent error

---

### Risk 2: Low Tradie Adoption
**Description:** Tradies are notoriously slow to adopt new tech. Despite "no app" promise, they may still prefer old methods.

**Triggers:**
- Skepticism about AI accuracy
- Habit of doing quotes the old way
- Fear of looking "unprofessional" using a chat bot
- Bad experience with previous software

**Mitigation Strategy:**
1. **Zero friction onboarding** — no download, no signup, just chat
2. **First 3 quotes free** — let them experience value before paying
3. **Trade influencer testimonials** — respected tradies vouching for it
4. "**I used to spend 2 hours per quote. Now it's 10 minutes.**" — real stories
5. **Referral incentives** — tradies trust other tradies more than ads

**Contingency:**
- If adoption stalls, pivot to white-label for trade businesses (B2B2C)
- Partner with Bunnings trade counter staff to recommend in-person

---

### Risk 3: Competitor Feature Creep
**Description:** Buildxact, Tradify, or ServiceTitan add a WhatsApp quoting bot that replicates our core value prop.

**Triggers:**
- Competitor sees our traction
- AI tooling becomes commoditised
- Big platform has more engineering resources

**Mitigation Strategy:**
1. **Own the chat layer** — be "the" WhatsApp quoting assistant, not "a" feature
2. **Personal RAG moat** — generic competitor can't replicate per-tradie intelligence quickly
3. **Speed** — ship fast, iterate faster than big platforms
4. **Community + brand** — "Australian tradies use Don't Quote Me" — cultural lock-in

**Contingency:**
- If Buildxact launches competing feature, double down on personalisation + human review tier
- Position as "specialist" vs. their "generalist" — we do one thing extremely well

---

### Risk 4: Supplier API Instability
**Description:** Bunnings Partner API goes down, changes pricing model, or revokes access.

**Triggers:**
- Bunnings API maintenance or deprecation
- Rate limiting during high volume
- Partnership terms change

**Mitigation Strategy:**
1. **Cached catalogue** — always have last-known prices as fallback
2. **Multi-supplier strategy** — Mitre 10, Reece, Tradelink as backups
3. **Graceful degradation** — if API fails, quote still generates with cached prices + warning
4. **Direct relationship** — meet Bunnings trade team in person; become preferred partner

**Contingency:**
- If Bunnings API permanently unavailable, pivot to web scraping (Oxylabs) + manual catalogue updates
- Accept higher error margin and flag it clearly

---

### Risk 5: Liability / Legal Challenge
**Description:** Tradie relies on our quote, job goes over budget, customer sues. Or regulator questions AI-generated quotes.

**Triggers:**
- Significant under-quote on materials
- Missing labour component
- Wrong specifications leading to rework
- Consumer law complaint about quote accuracy

**Mitigation Strategy:**
1. **Clear disclaimers on every quote** — "Draft for review — verify before ordering"
2. **Tradie confirmation** — they must explicitly approve before sending to customer
3. **Professional indemnity insurance** — $2M+ coverage
4. **Liability cap in ToS** — max liability = quote fee paid
5. **Human review for high-value jobs** — additional safety net

**Contingency:**
- If sued: Insurance covers legal defence; offer goodwill settlement
- If regulatory inquiry: Engage trade lawyer; demonstrate human oversight and disclaimers

---

### Risk 6: Data Breach / Privacy Violation
**Description:** Job site photos (including customer homes) leaked. Personal or business data exposed.

**Triggers:**
- Database vulnerability
- Misconfigured cloud storage
- Insider threat
- Third-party vendor breach

**Mitigation Strategy:**
1. **Auto-delete media** — 24–48 hours after quote completion
2. **Row-level security** — each tradie only accesses own data
3. **Encryption** — at rest and in transit
4. **AU data residency** — AWS Sydney / Supabase AU region
5. **Regular penetration testing** — quarterly
6. **SOC 2 Type II** — target certification Year 2

**Contingency:**
- Incident response playbook (see 09-operations-and-support.md)
- 72-hour notification to OAIC and affected users
- Immediate vulnerability patching + forensic audit

---

### Risk 7: LLM Cost Inflation
**Description:** Claude/GPT API costs rise, or model performance degrades with cheaper alternatives.

**Triggers:**
- Anthropic/OpenAI price increases
- High token usage per quote (complex jobs)
- Rapid user growth outpacing cost optimisation

**Mitigation Strategy:**
1. **Multi-model fallback** — Claude primary, GPT-4o fallback, DeepSeek/Grok for cost optimisation
2. **Cost monitoring** — LangSmith + Helicone track per-quote cost
3. **Token optimisation** — system prompt compression, efficient RAG retrieval
4. **Caching** — common job templates pre-computed where possible

**Contingency:**
- If costs exceed $1/quote, introduce "complex job surcharge" ($10 extra for >20-item BOMs)
- If costs spike 3x, pause marketing and optimise architecture before resuming growth

---

### Risk 8: WhatsApp / Telegram API Changes
**Description:** Meta or Telegram changes API terms, pricing, or functionality that breaks our core experience.

**Triggers:**
- Meta increases WhatsApp Business API fees
- Telegram introduces limits on bot messages
- Platform policy changes (e.g., restrictions on AI bots)

**Mitigation Strategy:**
1. **Dual channel** — maintain both WhatsApp and Telegram simultaneously
2. **Abstraction layer** — backend doesn't care which channel; easy to add new ones
3. **Direct relationships** — work with WATI, Twilio, and Meta partners for early access to changes

**Contingency:**
- If one platform becomes unusable, shift 100% to the other within 48 hours
- If both are restricted, pivot to SMS + simple web form (ugly but functional)

---

### Risk 9: Human Reviewer Bottleneck
**Description:** Complex jobs spike, human reviewers can't keep up, tradies wait >30 minutes.

**Triggers:**
- Marketing campaign drives influx of complex jobs
- Key reviewer sick/on leave
- Reviewer turnover

**Mitigation Strategy:**
1. **Multiple reviewers on retainer** — 3–5 part-time estimators
2. **Async queue** — tradie can leave and come back; quote ready when they return
3. **Escalation to senior** — if queue >10 jobs, founder/team lead steps in
4. **Compensation** — reviewers paid per review + quality bonus

**Contingency:**
- If reviewer availability drops below 80% during business hours, temporarily raise human review threshold to >$8k
- Communicate proactively: "High demand — your quote will be ready within 30 minutes"

---

### Risk 10: Material Price Volatility
**Description:** Bunnings prices change between quote generation and job acceptance. Tradie orders materials and costs are different.

**Triggers:**
- Supply chain disruptions
- Inflation spikes
- Seasonal demand (e.g., timber after bushfires)

**Mitigation Strategy:**
1. **Real-time disclaimers** — "Prices current as of [time] — verify before ordering"
2. **Quote validity period** — 30 days standard; 7 days for volatile materials
3. **Provisional sums** — agent suggests provisional sums for known volatile items
4. **Price alert** — if major price change detected, notify tradie who quoted recently

**Contingency:**
- If volatility exceeds 20% week-over-week, temporarily extend quote validity to 7 days for all jobs
- Offer "price lock" feature (premium tier) where we monitor prices for 30 days

---

## Risk Monitoring Dashboard

| Metric | Alert Threshold | Action |
|---|---|---|
| Quote accuracy complaints | >2% of quotes | Immediate product review + prompt audit |
| Human review queue | >10 jobs | Page additional reviewers + raise threshold |
| API error rate | >5% | Switch to fallback + investigate |
| LLM cost per quote | >$0.80 | Cost optimisation sprint |
| Churn rate | >10% monthly | Retention intervention + user interviews |
| Support ticket volume | >5% of active users | Identify root cause + fix |
| Data breach indicators | Any anomaly | Immediate security investigation |

---
