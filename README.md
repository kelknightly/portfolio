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
| [The Morning Digest](./morning-digest.md) | Personal | Live; printing paused | A two-page morning newspaper of household finances and AI news, written by an open-weight model (Gemma 4) chosen after a hand-checked comparison with Qwen 3.5, built to print at 6am and published by a cloud job every morning |
| [Sci-Fi Bot (@scifisanity)](./scifi-sanity-bot.md) | Personal | Retired (2020–2021, 2,456 posts) | A pre-AI Twitter bot that wrote sci-fi story seeds from 53 hand-built sentence structures and a 3,834-entry vocabulary in Sanity; featured in Sanity's Community Digest |
| [Spanish Flashcards](./spanish-flashcards.md) | Personal | Live | Interactive language learning app |
| [Crisis Storylines — Global Visualiser](./crisis-storylines-global.md) | Personal | Live | Interactive dashboard for exploring 3,158 global disaster events with maps, causal graphs, and AI-generated storylines |
| [LLM Hazard Response Equity Audit](./llm-hazard-equity-audit.md) | Personal / Research | Complete | Reproducible benchmark measuring whether 8 LLMs give worse emergency guidance to users in low-resource infrastructure contexts — 800 evaluated responses, pre-registered rubric, and a forensic record of judge hallucination caught mid-eval |

### Automation & data pipelines

| Project | Built for | Status | What it is |
|---|---|---|---|
| [RevOps Automation](./amazeeio-revops-automation.md) | amazee.io | Live and maintained | n8n workflows and scripts connecting product signups, outbound sequences, events, Drupal.org AI demos and Drupal AI trials into HubSpot, with Slack updates along the way |
| [Customer Conversation Insights](./customer-conversation-insights.md) | amazee.io | Delivered; ran on a schedule | Notion AI reads recorded customer meetings on a schedule and sorts them into recurring prospect questions with suggested answers for TAMs and CEMs, and content briefs for Marketing |

### Systems & operations

| Project | Built for | Status | What it is |
|---|---|---|---|
| [HubSpot Instance Merger](./amazeeio-hubspot-merger.md) | amazee.io | Delivered | CRM consolidation from Amazee Labs into amazee.io without disrupting live automations |
| [Documentation Transformation](./amazeeio-documentation.md) | amazee.io | Delivered | Full documentation strategy, platform evaluation, and migration from Confluence + MkDocs to Notion |
| [Sales Engagement Platform Evaluation](./sales-platform-evaluation.md) | amazee.io | Delivered | 20 prospecting and outreach platforms scored on 8 criteria, then vendor calls, demos and trials, ending in Amplemarket replacing Sales Navigator and Dripify |
| [Workstream Automations](./workstream-automation-wishlists.md) | amazee.io | Live process | Shared automation wishlists with each team, matched to the right tool: Marketing's 23-item list and a Notion → HubSpot event sync that writes the HubSpot ID back to Notion; BizOps' month-end data pulls from four Xero entities, now handled by a script after a build-vs-buy comparison; and BambooHR reporting moved into a private HR dashboard in Notion |
| [BizDev Notion Overhaul](./bizdev-notion-overhaul.md) | amazee.io | Delivered | Zero-loss restructure of the BizDev workspace into 7 linked Notion databases, with a guarded write path, audit ledger, rollback runbook and a verifier proving nothing was lost |

### Enablement & training

| Project | Built for | Status | What it is |
|---|---|---|---|
| [AI Enablement & Training](./ai-enablement.md) | amazee.io, DGXC, Mirantis | Ongoing | A self-serve Agentic Engineering knowledge base with my own recorded walkthroughs; a starter kit template repo whose agent instructions enforce security, auth and deployment standards from the first prompt; recorded workshops and 1:1 coaching from setup to production; Prompt to Prod, a weekly AI showcase I created and facilitate; and work to open the Mirantis AI Team weekly to newcomers and women |

### Data visualisation (earlier work)

Published on [Tableau Public](https://public.tableau.com/app/profile/kelly.quantrill/vizzes). Most of these come from before LLMs did this kind of work for you (the Star Trek series dates from late 2023), so the text analysis was done in Python and the data was gathered and checked by hand.

| Viz | What it is |
|---|---|
| [Star Trek Discovery Content Analysis (Seasons 1 & 4)](https://public.tableau.com/app/profile/kelly.quantrill/viz/StarTrekDiscoveryS1S2characterwordcountWIP/Dashboard1-total) | Who gets the most dialogue: transcripts from chakoteya.net, word counts per character computed in Python, shown as a treemap you can step through by series, season and episode |
| [Star Trek Discovery Sentiment Analysis (Season 1)](https://public.tableau.com/app/profile/kelly.quantrill/viz/StarTrekDiscoverySentimentAnalysis_17031176372700/Dashboard1) | A sentiment score for every line of dialogue in Season 1, plotted per episode with the episode average, so you can browse the emotional shape of the season line by line |
| [The Women of Star Trek Discovery](https://public.tableau.com/app/profile/kelly.quantrill/viz/TheWomenofStarTrekDiscovery/Dashboard1) | Share of dialogue spoken by women in each episode of Seasons 1 and 4, per-character figures, and the most-used words. The Bechdel test results come from my own reading of the online transcripts for every episode of Seasons 1–3: 37 of 40 passed, 5 of them only loosely |
| [Exploring Gendered Patterns in Spanish Hazard Words](https://public.tableau.com/app/profile/kelly.quantrill/viz/ExploringGenderedPatternsinSpanishHazardWords/Suddennessscoreintensitybygender) | Spanish words for hazards scored from 1 (slowest) to 5 (most sudden) and compared by grammatical gender. The slowest hazards were mostly feminine nouns, and the most sudden leaned masculine |

---

## MCP servers I work with

MCP (Model Context Protocol) servers let an AI agent like Claude use a tool directly: read the CRM, query a database, update a page. These are the ones I've used in real work, and what for.

| MCP server | What Claude does with it | Where it shows up |
|---|---|---|
| **AmpleMarket** | Reads my lead lists, researches and enriches each lead, writes personalised openers, and finds lookalike companies verified through AmpleMarket's own data | [AmpleMarket Team Hub](./amplemarket-team.md) |
| **HubSpot** | Searches and queries CRM records and properties, pulls marketing email, content and campaign attribution analytics, and checks the schema before I build anything that writes to it | [HubSpot Sales Triage Tool](./hubspot-sales-triage-tool.md), CRM audits |
| **Notion** | Searches, reads, updates and moves pages, and queries databases, so documentation and team hubs stay current without copy-and-paste | [AI Platform Operations Console](./flightdeck.md), workspace and documentation upkeep |
| **BigQuery** | Queries the Google Analytics 4 export directly while I designed the dashboard's data pipeline | [Mammoth Marketing Dashboard](./mammoth-marketing-dashboard.md) |
| **An internal deploy agent** | Asks for a deployment in plain words, gets a plan back, and applies it only after I approve | [AI Platform Operations Console](./flightdeck.md) |
| **Playwright** | Drives a real browser to test the dashboards I build | [HubSpot Sales Triage Tool](./hubspot-sales-triage-tool.md), [HubSpot Growth Dashboard](./hubspot-growth-dashboard.md) |
| **shadcn/ui** | Looks up and adds UI components from the shadcn registry | [HubSpot Sales Triage Tool](./hubspot-sales-triage-tool.md), [HubSpot Growth Dashboard](./hubspot-growth-dashboard.md) |
| **Next.js DevTools** | Reads build errors, routes and runtime state from the running Next.js app | [HubSpot Sales Triage Tool](./hubspot-sales-triage-tool.md), [HubSpot Growth Dashboard](./hubspot-growth-dashboard.md) |
| **Claude Docs** | Drafts and edits shared documents that people comment on | Reports and write-ups |

I use them across Claude Code, Cursor and OpenCode. Where an MCP server isn't available or isn't the right fit, I call the REST API directly. The Notion, Slack and BambooHR scripts behind several projects here do exactly that.

---

## How I work

My approach combines systems analysis with agentic development. I use AI — primarily Claude — as a core part of the build process, working iteratively in VS Code to move from problem definition to working software fast. I use n8n for automation workflows, webhooks and APIs to connect tools like HubSpot, Notion, Jira, and Google Sheets, and I lean on Next.js, TypeScript, PostgreSQL, and Supabase as my primary stack.

The speed this enables is real: a functioning dashboard in a week, an MVP in a weekend. That's not about cutting corners — it's about having a clear model of the problem before writing any code, and not getting in your own way once you do.

I also teach this way of working. At amazee.io I built an Agentic Engineering knowledge base and starter kit and run recorded workshops and 1:1s for workstream leads; for DGXC I created and facilitate Prompt to Prod, a weekly AI showcase. More in [AI Enablement & Training](./ai-enablement.md).

---

## Background

Before building these tools I worked as a data scientist, business analyst, and managing partner of my own digital agency. I've run documentation audits, led platform migrations, owned CRM instances, and designed the operational systems that keep teams moving. That background shapes how I approach every project: start with the problem, understand the constraints, then build the smallest thing that solves it properly.

---

## Tools I use

**Languages**
TypeScript, Python, JavaScript / Node.js, HTML, CSS

**Frameworks & Libraries**
Next.js, React, FastAPI, Prisma, Tailwind CSS, shadcn/ui, Recharts, React Leaflet, react-force-graph-2d

**Databases & content backends**
PostgreSQL, Supabase, Neon, LanceDB, Sanity

**AI & LLMs**
Claude (Anthropic), Claude Code scheduled tasks, OpenClaw, custom AI agents, Gemini (Google), Gemma 4 and Qwen 3.5 (open-weight, via Ollama), GPT-4o (OpenAI), OpenRouter, LiteLLM, promptfoo, sentence-transformers, Notion AI, HubSpot Breeze AI, sentiment analysis, retrieval-augmented generation, vector databases

**MCP servers**
AmpleMarket, HubSpot, Notion, BigQuery, Playwright, shadcn/ui, Next.js DevTools, Claude Docs, and an internal deploy agent

**CRM & Business Platforms**
HubSpot, Notion, Jira / Jira Service Management, Slack, AmpleMarket, Zoom Revenue Accelerator, BambooHR

**Automation & Pipelines**
n8n, GitHub Actions, Webhooks

**Data & APIs**
GDELT, Open-Meteo, Leaf API, NewsAPI, OilPriceAPI, ICE Data, GTFS-Realtime, Google Maps / Places, Google Analytics, Matomo, Webtrekk

**Dev & Deployment**
VS Code, Cursor, Git / GitHub, Docker, Vercel, nginx, systemd timers, WeasyPrint, CUPS

**Documentation**
Confluence, MkDocs, GitBook, Notion, Markdown

**Design & Visualisation**
Tableau, Leaflet.js, GeoJSON, Recharts, Force-directed graphs

**Open Source**
Ubuntu, OpenClaw, amazee.io/amazee.ai, Hugging Face, Llama 3.1, Next.js, React, FastAPI, Prisma, Tailwind CSS, shadcn/ui, Recharts, React Leaflet, react-force-graph-2d, n8n, promptfoo, Docker, nginx, MkDocs