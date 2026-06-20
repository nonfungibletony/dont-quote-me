# 02 — Product Specification

## Core Philosophy

"Text your quoting assistant like you text your apprentice — it does 90% of the work, you just confirm."

The experience must feel like having a super-competent virtual employee, not using software. No dashboards. No forms. Just chat.

## User Personas

### Primary: Solo Tradie (Electrician / Plumber / Carpenter)
- Busy on-site, dirty hands, bad signal
- Hates admin, lives in WhatsApp/Telegram
- Quotes 10–30 jobs/month
- Needs speed above all else

### Secondary: Small Team Lead (2–5 staff)
- Delegates quoting but still reviews everything
- Wants consistency across team quotes
- Needs brand/logo on PDFs
- Values win-rate data

## Exact Workflow (Per Quote)

### Step 1: Start a New Quote (30 seconds)
- Tradie opens "Don't Quote Me" chat (WhatsApp/Telegram bot)
- Types `/new` or taps "New Quote" button
- Instantly creates new chat thread titled: `Quote – [Customer Name] – [Date]`
- Agent greets: *"Hi [Name], ready when you are. Send voice note, photos, or quick description."*

### Step 2: On-Site Capture (Natural & Fast)
- Tradie sends: voice note (preferred), photos, short video walk-through, text notes
- **Burst input allowed** — send everything at once, no need to wait between messages
- Agent immediately acknowledges: *"Got it — 12 photos + 47-second voice note. Analysing now…"*

### Step 3: Agent Does the Heavy Lifting (Proactive, 20–40 seconds)
- Agent analyses everything (vision + voice transcription)
- RAG lookup: applies tradie's personal preferences, past job patterns, margins
- Agent replies with:
  1. One-sentence job summary + key assumptions (e.g. *"Assuming standard materials and single-phase power"*)
  2. Clean Bill of Materials (grouped by category)
  3. Quantity estimates
  4. Priced total (materials + labour + margin + GST)
  5. Simple 3D sketch preview or description
  6. *"This is a draft — happy to tweak anything."*
- **Max 2–3 smart clarification questions at a time** (never a laundry list)

### Step 4: Collaborative Fine-Tuning (Chat = Control)
- Tradie replies conversationally:
  - *"Add under-bench lighting"*
  - *"Make the cabinets premium instead of standard"*
  - *"Remove the splashback, they're keeping the old one"*
- Agent instantly updates draft, **highlights what changed**, shows new total
- Tradie says *"looks good"* or keeps tweaking

### Step 5: Finalise & Deliver (One Tap)
- Tradie types *"Finalise"* or taps button
- Agent generates polished PDF:
  - Branded with tradie's logo, contact details, terms
  - Validity period, assumptions clearly stated
  - GST breakdown
- Sends PDF back in chat
- Offers: *"Want me to send this straight to the customer?"*
- Chat thread auto-archived and linked to job for future reference

## Key Product Principles

| Principle | Rule |
|---|---|
| **Speed first** | Agent always replies fast with a draft — never leaves user waiting for questions |
| **Burst input** | Tradie dumps everything at once; agent handles messy input gracefully |
| **Smart defaults** | Agent makes educated guesses and flags them clearly |
| **Human safety net** | Jobs >$5k or flagged as complex → auto human review (invisible to user) |
| **Session smarts** | Easy `/duplicate`, `/resume [job name]`, searchable archive |
| **Offline-friendly** | Voice/photos queue and send when signal returns |
| **Branding & trust** | White-label option: "Quote Assistant for [Your Company]" |

## Feature List (MVP)

### Must-Have
- [ ] WhatsApp Business API + Telegram Bot API integration
- [ ] `/new` command for new quote threads
- [ ] Voice note transcription (OpenAI Whisper or built-in)
- [ ] Photo/video analysis (multimodal vision)
- [ ] BOM generation with quantities
- [ ] Real-time Bunnings pricing via Partner API
- [ ] Draft quote with assumptions flagged
- [ ] Conversational fine-tuning (add/remove/upgrade items)
- [ ] Branded PDF generation
- [ ] GST-inclusive pricing
- [ ] Session persistence (pause/resume quotes)
- [ ] Basic RAG memory (per-tradie preferences, margins)

### Should-Have (Phase 2)
- [ ] 3D sketch rendering (image generation first, then Meshy.ai)
- [ ] Human review queue for >$5k jobs
- [ ] Monthly insights report (win/loss rates, margin trends)
- [ ] Quote duplication (`/duplicate`)
- [ ] Searchable quote archive
- [ ] Direct customer send option
- [ ] One-tap "Apply my usual rules"

### Could-Have (Phase 3)
- [ ] Multi-supplier pricing (Mitre 10, etc.)
- [ ] Xero/MYOB export
- [ ] White-label for larger trade businesses
- [ ] Auto-create job in existing system post-acceptance
- [ ] Anonymised benchmarks: "Tradies in Sydney with similar job mix win 8% more by adding provisional sums here"
- [ ] Community features (private tradie-only tips group)

## Constraints & Guardrails

- **Max 2–3 clarification questions** at a time
- **Never guarantee prices** — always note *"Prices current as of [time]"*
- **Protect customer privacy** — media auto-deleted after processing
- **If input is insufficient**, politely ask for missing info rather than guessing wildly
- **For specialist/unusual jobs**, ask targeted questions
- **Always include GST** in final pricing
- **Apply tradie's standard margins** unless they say otherwise

## Output Style for Drafts

- Clean tables or bullet lists
- Subtotals clearly shown: Materials | Labour | Margin | GST | Total
- Flag every major assumption
- Keep replies under 400 words when possible
- Professional but not corporate tone

## Success Metrics

| Metric | Target (MVP) | Target (Phase 2) |
|---|---|---|
| First draft delivery time | <60 seconds | <45 seconds |
| Quote accuracy (material quantities) | ~70-85% | ~90%+ |
| Tradie satisfaction (NPS) | >40 | >50 |
| Conversion lift vs. old method | +10% | +15-20% |
| Time saved per quote | 2-3 hours | 3-4 hours |
| Human review rate | <20% of quotes | <10% |
| Monthly churn | <10% | <5% |
