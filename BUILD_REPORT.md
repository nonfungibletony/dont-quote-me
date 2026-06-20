# BUILD_REPORT.md — Wedge v1

**Project:** Don't Quote Me (Wedge v1)  
**Created:** 2026-06-20  
**Orchestrator:** Hermes orchestrator profile  

---

## Spec Decision Summary

### Accepted from User Proposal
- v1 scope: editable BoM only (no pricing, no PDF, no Bunnings API)
- Form-based capture (not chat) for structured input
- 5-10 sparkies from founder's network for validation
- Free during validation phase
- Stack: Next.js 15 + Drizzle + Neon + Anthropic Claude

### Refined / Added
- Voice transcription (STT) added to scope — essential for "messy raw material" value prop
- Instrumentation from Day Zero — events for job creation, BoM builds, edits, exports
- Parts catalogue sourcing explicitly defined (50+ item seeded catalogue)

### Deferred to Future Phases
- Chat channels (WhatsApp/Telegram)
- Live vendor pricing (Bunnings Partner API)
- Waste/quantity optimisation
- Learning/personalisation model
- Customer-facing priced quotes
- PDF generation
- Billing/paywall

---

## Task Map

| ID | Task | Assignee | Status |
|----|------|----------|--------|
| T1 | Project Setup | builder | todo |
| T2 | Database Schema | builder | todo |
| T3 | Seed Parts Catalogue + Sample Job | builder | todo |
| T4 | Build Capture Form UI | builder | todo |
| T5 | STT Integration + Audio Storage | builder | todo |
| T6 | BoM Build Endpoint | builder | todo |
| T7 | BoM Review UI | builder | todo |
| T8 | Theme + UI Polish | builder | todo |
| T9 | E2E Smoke Test | tester | todo |
| T10 | Validation Harness | tester | todo |
| T11 | Gate — Review and close | orchestrator | todo |

---

## Validation Plan (D16)

**Target:** 5-10 sparkies from founder's direct trade network  
**Method:** Build BoMs from their own real jobs, return to do it again, at least some send the resulting quote  
**Metric:** Repeat builds + quote exports tracked via instrumentation  
**Price:** Free during this phase (per D18)

---

## Hypothesis Under Test (D19)

> The AI's BoM is accurate enough that a sparky will stake their margin on it.

If this is false, nothing downstream (pricing, optimisation, learning) matters. v1 is deliberately shaped to expose this assumption fast and cheaply.

---

## Tech Stack

- **Frontend:** Next.js 15 (Turbopack), React 19, TypeScript, Tailwind, Shadcn UI
- **Auth:** Auth.js (credentials)
- **DB:** Drizzle ORM + Neon Postgres
- **LLM:** @anthropic-ai/sdk (Claude Sonnet)
- **Storage:** Vercel Blob (photos/audio)
- **STT:** Hosted API (Deepgram or similar)
- **Package manager:** bun