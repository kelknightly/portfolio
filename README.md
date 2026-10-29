# Portfolio

#### [LinkedIn: Kelly Knight](https://www.linkedin.com/in/kelly-knight/)

I'm an AI & Business Systems Analyst and Agentic Workflow Architect. My work sits at the intersection of systems thinking and hands-on building: I find where a business is losing time or working around broken processes, design the solution, and build it — whether that's a custom internal tool, an automated data pipeline, a CRM migration, or an organisation-wide documentation overhaul.

The work here spans two main clients — amazee.io (a managed cloud hosting and professional services company) and DGXC (a specialist consultancy that builds intelligent middleware) — alongside a handful of personal projects I built for myself or to scratch a research itch.

Most of it was built solo. Most of it is live and in use.

---

## What's in here

Each project has its own case study. They're written honestly — including what was hard, what I'd do differently, and whether the thing is live or still in progress.

### Custom tools

| Project | Built for | Status | What it is |
|---|---|---|---|
| [AI Platform Operations Console](./flightdeck.md) | amazee.io | Live, in active development | Air-traffic-control-style operations console for an AI inference platform on live GPU racks, with an alert "doorman" (rules first, then Jev, a small decision model) that groups alert storms before agents start, evaluated against Claude Haiku and Sonnet |
| [Support Relay](./support-relay/support-relay.md) | amazee.io | In active development | AI-powered support dashboard — tickets, customer context, and drafted responses in one place |
| [HubSpot Sales Triage Tool](./hubspot-sales-triage-tool.md) | amazee.io | Live | My first vibe-coded project; daily lead prioritisation for the sales team |
| [HubSpot Growth Dashboard](./hubspot-growth-dashboard.md) | amazee.io | Live | Deal and lead visibility for leadership |
| [Mammoth Marketing Dashboard](./mammoth-marketing-dashboard.md) | amazee.io | Live | Marketing analytics for PLG and Enterprise: traffic, conversions, ad spend and CRM pipeline from Matomo, GA4/BigQuery, Google Ads and HubSpot, refreshed daily |
| [Marketing / BizDev / Sales Sync Hub](./marketing-sync-hub.md) | amazee.io | Live | Live meeting dashboard replacing static slide decks |
| [AmpleMarket Team Hub](./amplemarket-team.md) | amazee.io | In progress | Outreach research and workflow hub; Claude works lead lists through AmpleMarket's MCP server, and replies flow into HubSpot |
| [Customer Slack Pulse Agent](./customer-recaps.md) | amazee.io | Live | Weekly AI-generated digest of all customer Slack channels, written to Notion and synced to HubSpot deals |
| [User Adoption Tracker](./tracking-user-adoption.md) | amazee.io | Live | Weekly DAU/WAU/MAU metrics for internal tools, written to Notion |
| [Customer Command Centre (CCC)](./ccc-jira-dashboard/ccc-jira-dashboard.md) | amazee.io | Superseded | Jira-to-customer operational dashboard |
| [CEM Onboarding Tool](./cem-onboarding-tool.md) | amazee.io | Live prototype | Onboarding journey visualiser |
| [FuelSignal MVP](./fuelsignal-mvp.md) | DGXC | MVP / Lead gen | Fuel procurement intelligence platform |
| [Urban Resilience Sentinel](./urban-resilience-sentinel.md) | Personal | In progress | Urban data archiving engine |
| [Spanish Flashcards](./spanish-flashcards.md) | Personal | Live | Interactive language learning app |
| [Crisis Storylines — Global Visualiser](./crisis-storylines-global.md) | Personal | Live | Interactive dashboard for exploring 3,158 global disaster events with maps, causal graphs, and AI-generated storylines |
| [LLM Hazard Response Equity Audit](./llm-hazard-equity-audit.md) | Personal / Research | Complete | Reproducible benchmark measuring whether 8 LLMs give worse emergency guidance to users in low-resource infrastructure contexts — 800 evaluated responses, pre-registered rubric, and a forensic record of judge hallucination caught mid-eval |

### Automation & data pipelines

| Project | Built for | Status | What it is |
|---|---|---|---|
| [RevOps Automation](./amazeeio-revops-automation.md) | amazee.io | Live and maintained | n8n workflows and scripts connecting product signups, outbound sequences, events, and AI trials into HubSpot |

### Systems & operations

| Project | Built for | Status | What it is |
|---|---|---|---|
| [HubSpot Instance Merger](./amazeeio-hubspot-merger.md) | amazee.io | Delivered | CRM consolidation from Amazee Labs into amazee.io without disrupting live automations |
| [Documentation Transformation](./amazeeio-documentation.md) | amazee.io | Delivered | Full documentation strategy, platform evaluation, and migration from Confluence + MkDocs to Notion |
| [BizDev Notion Overhaul](./bizdev-notion-overhaul.md) | amazee.io | Delivered | Zero-loss restructure of the BizDev workspace into 7 linked Notion databases, with a guarded write path, audit ledger, rollback runbook and a verifier proving nothing was lost |

---

## How I work

My approach combines systems analysis with agentic development. I use AI — primarily Claude — as a core part of the build process, working iteratively in VS Code to move from problem definition to working software fast. I use n8n for automation workflows, webhooks and APIs to connect tools like HubSpot, Notion, Jira, and Google Sheets, and I lean on Next.js, TypeScript, PostgreSQL, and Supabase as my primary stack.

The speed this enables is real: a functioning dashboard in a week, an MVP in a weekend. That's not about cutting corners — it's about having a clear model of the problem before writing any code, and not getting in your own way once you do.

I'm also currently running agentic engineering workshops at amazee.io, training workstream leads to bring AI into their own teams.

---

## Background

Before building these tools I worked as a data scientist, business analyst, and managing partner of my own digital agency. I've run documentation audits, led platform migrations, owned CRM instances, and designed the operational systems that keep teams moving. That background shapes how I approach every project: start with the problem, understand the constraints, then build the smallest thing that solves it properly.

---

## Tools I use

**Languages**
TypeScript, Python, JavaScript / Node.js, HTML, CSS

**Frameworks & Libraries**
Next.js, React, FastAPI, Prisma, Tailwind CSS, shadcn/ui, Recharts, React Leaflet, react-force-graph-2d

**Databases**
PostgreSQL, Supabase, Neon, LanceDB

**AI & LLMs**
Claude (Anthropic), OpenClaw, custom AI agents, Gemini (Google), GPT-4o (OpenAI), OpenRouter, LiteLLM, promptfoo, sentence-transformers, Notion AI, HubSpot Breeze AI, sentiment analysis, retrieval-augmented generation, vector databases

**CRM & Business Platforms**
HubSpot, Notion, Jira / Jira Service Management, Slack, AmpleMarket, Zoom Revenue Accelerator

**Automation & Pipelines**
n8n, GitHub Actions, Webhooks

**Data & APIs**
GDELT, Open-Meteo, Leaf API, NewsAPI, OilPriceAPI, ICE Data, GTFS-Realtime, Google Maps / Places, Google Analytics, Matomo, Webtrekk

**Dev & Deployment**
VS Code, Cursor, Git / GitHub, Docker, Vercel, nginx

**Documentation**
Confluence, MkDocs, GitBook, Notion, Markdown

**Design & Visualisation**
Tableau, Leaflet.js, GeoJSON, Recharts, Force-directed graphs

**Open Source**
Ubuntu, OpenClaw, amazee.io/amazee.ai, Hugging Face, Llama 3.1, Next.js, React, FastAPI, Prisma, Tailwind CSS, shadcn/ui, Recharts, React Leaflet, react-force-graph-2d, n8n, promptfoo, Docker, nginx, MkDocs