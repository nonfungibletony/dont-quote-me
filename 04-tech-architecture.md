# 04 — Technical Architecture

## High-Level Architecture

```
Tradie (on-site)
   ↓ (voice / photos / video / text)
WhatsApp Business API  or  Telegram Bot API
   ↓ (webhook)
Backend Orchestrator (FastAPI + LangGraph)
   ↓
Stateful Quote Agent (per-quote thread + per-tradie RAG)
   ├── Multimodal LLM (Claude 3.5/4 or GPT-4o / Gemini)
   ├── Tools:
   │   ├── Vision analysis
   │   ├── Bunnings Partner API (pricing/stock)
   │   ├── RAG memory (user history + preferences)
   │   ├── PDF generator
   │   └── 3D sketch renderer
   ↓
Response back to same chat thread + branded PDF
   ↓ (optional)
Human review queue (for >$5k or complex jobs)
```

The entire experience stays inside the tradie's existing chat app. **No new mobile app.**

## Detailed Tech Stack (MVP → Scale)

| Layer | Technology | Why | Alternatives |
|---|---|---|---|
| **Chat Interface** | WhatsApp Business API (via WATI or Twilio) + Telegram Bot API | Tradies already live in WhatsApp (AU #1). WATI/Twilio give reliable webhooks, templates, media support, AU compliance. | Respond.io, 360dialog, direct Meta BSP |
| **Backend / Orchestration** | Python + FastAPI + LangGraph | LangGraph is the 2026 gold standard for stateful, reliable agents. Handles conversation memory, tool calling, branching logic, per-quote sessions. | Node.js + LangGraph.js; n8n for no-code prototyping |
| **LLM / Reasoning** | Primary: Anthropic Claude 3.5 Sonnet or Claude 4; Fallback: GPT-4o or Gemini 2.0 Flash | Best reasoning + native multimodal. Excellent at structured output and conversational tone. | Grok-2 or DeepSeek V3 for cost optimisation later |
| **Multimodal Input** | Claude/Gemini vision + OpenAI Whisper (or built-in audio) | Handles blurry job-site photos, voice notes, short videos in one prompt. | Gemini 2.0 for strongest native video understanding |
| **Personalised Memory / RAG** | Supabase pgvector (or Pinecone) with per-user namespaces | Private RAG per tradie = the real moat. Every quote improves future accuracy. | Weaviate or ChromaDB for self-hosted |
| **Database** | Supabase (PostgreSQL + Auth + Storage + Edge Functions) | All-in-one: user accounts, quote history, payment records, media temp storage. Serverless + AU region. | Neon + PlanetScale |
| **File Storage** | Supabase Storage or AWS S3 (Sydney) | Temporary storage for photos/videos (auto-delete after 24–48 hrs). | Cloudinary for advanced image optimisation |
| **Real-time Pricing** | Bunnings Partner APIs (developer.live.bunnings.com.au) | Official partner access required. Falls back to cached catalogue + scraper for non-partner items. | EDI via SPS Commerce/Crossfire; Mitre 10 etc. via similar APIs |
| **PDF Generation** | WeasyPrint (Python) or pdf-lib + dynamic templates | Clean, branded PDFs with logo, terms, assumptions, GST breakdown. | DocRaptor or PDFMonkey |
| **3D Sketch / Schematic** | Short-term: Grok Imagine / Flux / Midjourney API; Long-term: Meshy.ai or Luma Dream Machine | "3D sketch" = rendered schematic image first (fast & cheap). Later upgrade to interactive GLB preview. | TripoSR for instant 3D from photos |
| **Payments** | Stripe (Subscriptions + Usage-based) | Pay-per-quote ($19–$49) or monthly unlimited. Webhooks for instant credit top-up. | Paddle for simpler AU tax handling |
| **Human-in-the-Loop** | Supabase queue + simple Retool / custom dashboard | Complex jobs (>$5k or flagged) auto-routed to human reviewer in same chat thread. | Notion or Slack queue for v1 |
| **Monitoring / Observability** | LangSmith + Helicone + OpenTelemetry | Track every LLM call, cost, latency, failure rate. Critical for controlling token spend. | — |
| **Hosting / Infra** | Fly.io or Render (global) + AWS Sydney for data residency | Low latency for AU tradies, easy scaling, AU data compliance (APPA). | Vercel + Supabase edge for full serverless |

## Data Flow & Security

```
Tradie sends media
   ↓
Webhook → Backend downloads to Supabase Storage (temp)
   ↓
Agent triggers LLM analysis (vision + voice transcription)
   ↓
RAG lookup (user-specific vector store)
   → Applies personal margins, preferences, past job patterns
   ↓
Tools fire (Bunnings pricing)
   → Structured BOM + pricing generated
   ↓
Draft returned in chat
   → Tradie iterates conversationally
   ↓
On "Finalise"
   → PDF rendered → Sent in chat + stored in user history (future RAG)
   ↓
All media auto-deleted after processing + quote completion
```

### Security Highlights
- **End-to-end:** Media never stored long-term
- **Row-level security** in Supabase (each tradie only sees their own data)
- **AU data residency**
- **Clear disclaimers** on every quote ("Prices current as of X – always verify")

## LangGraph Orchestration Details

### Why LangGraph (Not Plain LangChain)
- **Stateful** — remembers everything in a quote session
- **Graph-based** — workflow as nodes + edges, not fragile prompt chains
- **Cycles & Human-in-the-Loop** — supports loops (fine-tuning) and branching to human review
- **Persistence** — saves entire graph state to Supabase; quotes can be paused and resumed
- **Observability** — LangSmith integration for debugging every step
- **Tool calling & structured output** — native, reliable support

### Core LangGraph Concepts

| Concept | What It Is | How We Use It |
|---|---|---|
| **Graph** | Directed graph of nodes and edges | The entire quoting workflow |
| **State** | Typed object flowing between nodes | `QuoteState` class: customer info, media list, current BOM, pricing, version history, assumptions, tradie preferences |
| **Node** | Function receiving state and returning updated state | `process_input`, `analyze_media`, `generate_draft`, `apply_user_changes`, `create_pdf` |
| **Edge** | Conditional routing (if/else) | "If draft ready → send to user. If user asks for change → loop back. If >$5k → human review" |
| **Persistence** | Checkpointing (save state to DB) | Quote paused mid-chat and resumed days later |
| **Interrupt** | Pause graph for human input | Human reviewer steps in for complex jobs |

### Recommended Graph Structure

```
Start New Quote
   ↓
Greet & Wait for Input
   ↓
Receive Media/Text
   ↓
Analyze Media + Voice (LLM + Vision)
   ↓
RAG Lookup (Personal Preferences)
   ↓
Generate First Draft (BOM + Pricing + Sketch)
   ↓
Send Draft to Tradie
   ↓
User Reply?
   ├── Change Request → Apply Changes + Recalculate → [loop back to Send Draft]
   ├── Finalise → Generate Branded PDF → Deliver PDF + Log Outcome
   └── Complex />$5k → Interrupt → Human Review → [back to Generate PDF]
   ↓
Save to RAG + Archive Session
```

### State Schema (Python)

```python
from langgraph.graph import StateGraph, END
from langgraph.checkpoint.supabase import SupabaseSaver
from typing import TypedDict

class QuoteState(TypedDict):
    quote_id: str
    tradie_id: str
    messages: list          # chat history
    media: list             # photo/video URLs
    current_bom: dict
    total_price: float
    assumptions: list
    version: int
    needs_human_review: bool
```

### How a Real Quote Session Works

1. Tradie sends `/new` → New graph instance with unique `thread_id` (quote ID)
2. Every WhatsApp message triggers `app.invoke()` or `app.astream()` with new input
3. LangGraph automatically:
   - Loads latest state from Supabase
   - Runs appropriate nodes
   - Saves checkpoint after each major step
4. Fine-tuning loop = edge that routes back to draft node
5. Human review uses `interrupt_before=["pdf"]` so reviewer can jump into same thread

## MVP vs. Scale Roadmap

### MVP (4–6 weeks)
- WATI + Supabase + LangGraph + Claude 3.5 + Stripe + basic PDF + image-based "3D sketch"
- One trade focus (e.g., electricians or kitchen renos)
- Basic RAG (margins, preferences)
- Manual Bunnings pricing fallback (cached catalogue)

### Phase 2 (3 months)
- Bunnings Partner API live
- Per-user RAG fully tuned
- Human review queue
- Monthly insights report
- 3D sketch rendering

### Phase 3 (6–12 months)
- Multi-supplier pricing
- 3D model export (interactive)
- White-label for larger trade businesses
- Xero/MYOB export
- Auto-ordering post-acceptance
- Expansion to NZ/UK markets

## Cost Estimate (Monthly, at Scale)

| Component | Estimated Cost |
|---|---|
| LLM API calls (Claude/GPT-4o) | $500–$2,000 / 10k quotes |
| WhatsApp Business API (WATI/Twilio) | $50–$300 / month |
| Supabase (DB + Auth + Storage) | $25–$150 / month |
| Hosting (Fly.io/Render) | $50–$200 / month |
| Bunnings API (partner tier) | Likely free or revenue-share |
| PDF generation (WeasyPrint) | Self-hosted, negligible |
| 3D rendering (image gen) | $200–$800 / month |
| Monitoring (LangSmith + Helicone) | $50–$200 / month |
| **Total (at 10k quotes/month)** | **~$1,000–$3,500** |
| **Revenue (at $29 avg/quote)** | **$290,000** |
| **Gross margin** | **~>98%** |

---
