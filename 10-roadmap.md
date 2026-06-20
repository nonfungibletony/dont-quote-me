# 10 — Product Roadmap

## MVP (Month 1–2: Build)

### Goal
Get a working quoting assistant into the hands of 5–10 real tradies for feedback.

### Features
- [ ] WhatsApp Business API integration (WATI)
- [ ] Telegram Bot API as backup
- [ ] `/new` command for new quote threads
- [ ] Voice note transcription (OpenAI Whisper)
- [ ] Photo analysis (Claude 3.5 Sonnet vision)
- [ ] Basic BOM generation with quantities
- [ ] Cached Bunnings pricing (manual import or basic scraper)
- [ ] Draft quote with assumptions flagged
- [ ] Conversational fine-tuning (add/remove items)
- [ ] Basic branded PDF generation (WeasyPrint)
- [ ] GST-inclusive pricing
- [ ] Session persistence (pause/resume quotes)
- [ ] Basic RAG memory (margins, preferences per tradie)
- [ ] Stripe billing (pay-per-quote)

### Tech
- FastAPI + LangGraph
- Supabase (DB + Auth + Storage)
- Claude 3.5 Sonnet
- WATI + Twilio
- WeasyPrint
- Stripe

### Trade Focus
- Electricians in Sydney metro
- Simple jobs: lighting, power points, switchboard upgrades

### Success Criteria
- 5 beta users generating real quotes
- First draft <2 minutes
- User can complete a quote end-to-end without human help (for simple jobs)
- No critical bugs

---

## Phase 2 (Month 3–5: Iterate & Scale)

### Goal
Add accuracy, speed, and intelligence. Expand to 100+ active users.

### Features
- [ ] Bunnings Partner API live (real-time pricing)
- [ ] Per-user RAG fully tuned (20–30 quotes per user = strong profile)
- [ ] Human review queue for >$5k jobs (Retool dashboard)
- [ ] 3D sketch rendering (image generation — Flux/Midjourney)
- [ ] Monthly insights report (win/loss tracking)
- [ ] Quote duplication (`/duplicate`)
- [ ] Searchable quote archive
- [ ] Direct customer send option
- [ ] One-tap "Apply my usual rules"
- [ ] Telegram as primary channel (if WhatsApp API is limiting)

### Trade Expansion
- Add plumbers (Sydney metro)
- Add kitchen/bathroom renovators

### Integrations
- [ ] Bunnings Partner API
- [ ] Basic Xero export (csv)

### Success Criteria
- 100 active tradies
- First draft <60 seconds
- >80% of simple quotes need zero human review
- NPS >40
- <10% monthly churn

---

## Phase 3 (Month 6–12: Grow & Monetise)

### Goal
Scale to 1,000+ active tradies. Add enterprise features. Prepare for Series A or profitability.

### Features
- [ ] Multi-supplier pricing (Mitre 10, Reece, Tradelink)
- [ ] 3D model export (interactive GLB preview)
- [ ] White-label for larger trade businesses
- [ ] Full Xero/MYOB integration
- [ ] Auto-ordering post-acceptance (supplier API)
- [ ] Team plans (up to 5 users)
- [ ] Anonymised benchmarking
- [ ] Community features (tradie tips group)
- [ ] Mobile app (optional — only if chat proves insufficient)

### Market Expansion
- Melbourne, Brisbane, Perth
- Builders, painters, general handymen
- NZ market (regulatory + pricing adaptation)

### Enterprise
- Custom branding
- API access for integrations
- Dedicated account manager
- Volume pricing

### Success Criteria
- 1,000+ active tradies
- $50k+ MRR
- First draft <45 seconds
- NPS >50
- <5% monthly churn
- Gross margin >95%

---

## Future Vision (Year 2+)

### Potential Directions
1. **Full Service-as-Software Trade Platform**
   - Not just quoting: scheduling, invoicing, job management — all via chat
   - "Your entire back office in one chat thread"

2. **Supplier Marketplace**
   - Tradies accept quote → one-click order from Bunnings/Mitre 10
   - Commission on orders (new revenue stream)

3. **Insurance & Finance**
   - Quote → instant trade insurance quote
   - Working capital loans based on quote pipeline

4. **International Expansion**
   - UK (similar trades, English-speaking, GBP pricing)
   - US (massive market, different supplier landscape)

5. **AI-Powered Apprenticeship**
   - Junior tradies learn by watching the agent build quotes
   - Training mode: "Why did you choose this material?"

---

## Milestone Timeline

| Month | Milestone |
|---|---|
| **M1** | Backend + WhatsApp integration live |
| **M2** | 5 beta users generating real quotes |
| **M3** | Bunnings API + human review queue |
| **M4** | 100 users, first revenue |
| **M5** | 3D sketch + monthly insights |
| **M6** | 500 users, $15k MRR |
| **M9** | 1,000 users, white-label pilot |
| **M12** | 3,500 users, $50k+ MRR, break-even or profitable |

---
