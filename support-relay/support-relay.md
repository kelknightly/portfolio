# Support Relay

> An intelligent support dashboard that eliminates context-switching for the amazee.io first-level support team — bringing tickets, customer context, and AI assistance into one place, with responses drafted before you've even read the thread.

| | |
|---|---|
| **Built for** | amazee.io |
| **Timeline** | ~1 week initial build, ongoing |
| **Status** | In active development — Phase 1 complete and functional |
| **Team** | Built on a foundation by another engineer; grown from there with engineering and security team input |
| **Stack** | Next.js 14, TypeScript, Python 3.11, FastAPI, PostgreSQL, pgvector, WebSockets, shadcn/ui, TailwindCSS |

---

## The problem

The amazee.io support team was living across four tools simultaneously: Jira Service Management for tickets, Slack for internal communication, Notion for customer context, and whatever browser tab they happened to have open for everything else. Every time an engineer opened a new ticket, they had to reconstruct the full picture manually — who is this customer, what tier are they on, who's their CEM, what do they use, what have we seen from them before?

That context-switching adds up. It slows response times, increases cognitive load, and means that the quality of a response often depends on how much time the engineer has to do the background work — not just how good they are.

## What I built

Support Relay is a unified support interface that does most of that background work automatically. When a ticket is opened, the dashboard pulls the full conversation thread from JSM, enriches it with customer context from Notion (tier, TAM, CEM, products in use, sentiment score from the CCC), and immediately begins generating an AI briefing — no button press required.

By the time the engineer has read the ticket, there's already an internal analysis and a customer-ready draft response waiting for them. The internal analysis and the customer-facing draft are kept visually separate and clearly labelled. If the engineer wants another draft option, they can request one without triggering a full re-analysis. When they're ready to reply, they can post directly to JSM from within the dashboard.

The AI runs through amazee.ai's LLM gateway, and each job is moving to the model that won it in a November 2026 evaluation (see [Choosing the models](#choosing-the-models)). The system also includes an editable organisational context document that the team can maintain themselves — product terminology, customer tiers, how to handle specific categories of issue — so the AI's responses stay accurate and on-brand over time.

### Sprot, the AI chat assistant

I also designed and built Sprot, an AI chat assistant that sits inside Support Relay. Engineers ask it questions in plain English, such as "why did this customer's bill spike last month?", "is this site's traffic a real problem or bot noise?" or "what's being discussed in this customer's Slack channel?", and it answers from live data rather than from a FAQ.

Sprot runs on Claude Sonnet through the amazee.ai gateway. I wrote the tool-calling loop myself instead of using a framework like LangChain. The model can call 32 tools across Jira Service Management, the customer database, Lagoon (deploy status, build logs, project settings), amazee.io's billing system, OpenSearch traffic logs, Slack (read-only) and a semantic search over our documentation. It chains them together when a question needs it: for example, it works out which billing account owns a set of projects, pulls each project's costs, then sums them up in one answer.

I built in some safeguards from the start:

- The only change Sprot can make in Jira is moving a ticket's status, and it confirms first. It can draft internal notes, but a person reviews and posts them.
- It reads Slack and never posts there.
- Onboarding data is only available to the Customer Success team, and that limit is enforced in three places, not just hidden in the interface.
- It never reveals credentials.

A plain-text reference file holds the billing knowledge Sprot needs, such as what each product code means, so the team can correct it without changing any code.

## How I built it

This project started from a foundation built by another engineer on the team. I took that foundation and grew it significantly — adding the AI agent layer, the Notion enrichment, the sentiment data feed from CCC, the multi-draft generation system, the JSM reply capability, and the Sprot chat assistant. I worked closely with both the engineering team (on architecture and API connections) and the security team (on data handling and access controls).

The AI layer is where most of the interesting technical work lives. The system uses **Retrieval-Augmented Generation (RAG)** to ground the AI's responses in real, current data rather than relying on model knowledge alone. When a ticket comes in, the ticket's own customer context — account tier, recent sentiment, products in use — is looked up directly and added to the prompt, along with the most relevant excerpts from the documentation. This means the AI is reasoning over actual information about this specific customer, and can cite the documents it relied on.

The vector database stores embedded versions of the public documentation and past tickets. When a ticket arrives, a similarity search surfaces the most relevant chunks, with a minimum similarity so that weak matches are dropped — more reliable than stuffing the entire knowledge base into every prompt. Scheduled jobs keep the index current. The evaluation below caught a problem here: customer profiles shared that index, so one ticket picked up another customer's profile. Ticket searches are being restricted to public documentation, with tests that fail if any search doesn't name its sources.

The internal analysis (what's actually going on here, what's the risk level, what do we know about this customer) and the customer-facing draft (what we say externally) are generated together from one model call with separate output rules, then kept visually separate in the interface. The draft has its own constraints: plain text, no internal details, and the team's voice and tone rules. I tested splitting them into two calls, and the result is in the section below.

Getting this to work reliably required careful prompt engineering, a well-structured indexing strategy, and iterative testing with the support team on real tickets. The editable organisational context document was important here too: giving the team ownership of what the AI knows means the system stays accurate as the product and processes evolve, without needing engineering intervention every time something changes.

My role throughout has been half product owner, half engineer: I decide what gets built, I build most of it, and I work with the team when I hit the edges of what vibe coding can do alone.

## Choosing the models

Everything first ran on one model, Claude Sonnet 4.5. In November 2026 I tested whether that was still right, running the real production prompts against 13 models for sentiment and 9 for the analysis and draft, all through the same private gateway with customer details replaced by placeholders. The whole thing took a day and cost about 26 USD.

I labelled 249 customer comments blind myself and rated 157 analysis-and-draft outputs on a page that hid which model wrote what. Claude Opus 5.5 judged 297 outputs across 50 tickets, and Kimi K3, from a different model family, re-checked 98 of them. I'm not a support engineer, so my ratings mostly reflect structure and tone: of 51 drafts the judge flagged as wrong or unsafe, I would have sent 36. A person reviewing for polish and a judge checking correctness catch different failures.

Support Relay is being updated so that each job calls a different model:

| Job | Was | Moving to | Why |
|---|---|---|---|
| Sentiment on every comment | Claude Sonnet 4.5 | **Gemma 4 31B** (open-weight), with a revised prompt | As accurate, about 30× cheaper (43 USD down to 1.45 USD per 10,000 comments), and on the same cloud provider |
| Internal analysis and customer draft | Claude Sonnet 4.5 | **Claude Sonnet 5.5**, with the token limit raised to 4,000 and the system prompt cached | 88% of drafts sendable against 48%, and internal details leaked in 0% of drafts against 15% |

Sprot and traffic analysis weren't part of this evaluation and stay on Claude Sonnet 4.5.

**The sentiment problem was the categories, not the model.** The old model matched a human label 44% of the time, and its at-risk flags were right about one time in four. Every model had the same problem. The ten categories mixed how a customer sounds with what is happening to them, so a calm "we're cancelling" had to be either neutral or churn risk. The revised prompt asks for a tone plus three yes-or-no flags: blocking, escalation and churn. On 100 fresh comments, tone accuracy with the same model rose from 38% to 64%.

**Cheaper options.** Splitting the work so Claude Haiku 5.5 writes the analysis and Sonnet 5.5 writes the draft costs about a third as much, with 82% of drafts sendable. Haiku 5.5 alone is 17× cheaper, with 70% sendable. Caching the 7,000-token system prompt cuts the cost of the analysis call by nearly a quarter.

The limits: one non-specialist human reviewer, 50 tickets for the shortlisted models and 20 for the rest, and no support engineer has checked the AI judges, because the tool has been quiet during a company merger.

## How long it took

The first functional version took about a week. The project is ongoing — each phase adds meaningful capability. Phase 1 (live now) covers the full ticket-to-response workflow. Future phases will deepen the AI agent capabilities and expand the integration surface.

## Where it stands now

Phase 1 is complete. The dashboard works end-to-end: tickets appear in real time via webhook, customer context enriches on every ticket open, and the AI panel generates its analysis and draft automatically. Sprot is live in production with all of its data sources connected. The team is actively using it.

This is the most technically complex thing I've built so far, and it's the one I'm most proud of. It's also the clearest demonstration of what happens when you combine a deep understanding of the problem (I sat with the support team and watched how they worked) with the ability to move fast in code.
