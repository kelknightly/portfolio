# Support Relay

> An intelligent support dashboard that eliminates context-switching for the amazee.io first-level support team — bringing tickets, customer context, and AI assistance into one place, with responses drafted before you've even read the thread.

| | |
|---|---|
| **Built for** | amazee.io |
| **Timeline** | ~1 week initial build, ongoing |
| **Status** | In active development — Phase 1 complete and functional |
| **Team** | Built on a foundation by another engineer; grown from there with engineering and security team input |
| **Stack** | Next.js 14, TypeScript, Python 3.11, FastAPI, PostgreSQL, WebSockets, shadcn/ui, TailwindCSS |

---

## The problem

The amazee.io support team was living across four tools simultaneously: Jira Service Management for tickets, Slack for internal communication, Notion for customer context, and whatever browser tab they happened to have open for everything else. Every time an engineer opened a new ticket, they had to reconstruct the full picture manually — who is this customer, what tier are they on, who's their CEM, what do they use, what have we seen from them before?

That context-switching adds up. It slows response times, increases cognitive load, and means that the quality of a response often depends on how much time the engineer has to do the background work — not just how good they are.

## What I built

Support Relay is a unified support interface that does most of that background work automatically. When a ticket is opened, the dashboard pulls the full conversation thread from JSM, enriches it with customer context from Notion (tier, TAM, CEM, products in use, sentiment score from the CCC), and immediately begins generating an AI briefing — no button press required.

By the time the engineer has read the ticket, there's already an internal analysis and a customer-ready draft response waiting for them. The internal analysis and the customer-facing draft are kept visually separate and clearly labelled. If the engineer wants another draft option, they can request one without triggering a full re-analysis. When they're ready to reply, they can post directly to JSM from within the dashboard.

The AI powering this is running through amazee.ai's LLM gateway (Claude Sonnet), and the system includes an editable organisational context document that the team can maintain themselves — product terminology, customer tiers, how to handle specific categories of issue — so the AI's responses stay accurate and on-brand over time.

## How I built it

This project started from a foundation built by another engineer on the team. I took that foundation and grew it significantly — adding the AI agent layer, the Notion enrichment, the sentiment data feed from CCC, the multi-draft generation system, and the JSM reply capability. I worked closely with both the engineering team (on architecture and API connections) and the security team (on data handling and access controls).

The AI layer is where most of the interesting technical work lives. The system uses **Retrieval-Augmented Generation (RAG)** to ground the AI's responses in real, current data rather than relying on model knowledge alone. When a ticket comes in, the relevant customer context — account tier, open issues, recent sentiment scores, product usage — is retrieved from the vector store and injected into the prompt alongside the ticket content. This means the AI is reasoning over actual information about this specific customer, not making inferences from general training data.

The vector database stores embedded representations of the organisational context document, customer records, and historical ticket patterns. When a ticket arrives, a similarity search runs against these embeddings to surface the most relevant chunks — which is more reliable than stuffing the entire knowledge base into every prompt, and keeps latency low. The indexing pipeline keeps the vector store current as the context document is updated by the team: changes are chunked, re-embedded, and written back to the index automatically, so the AI's knowledge doesn't go stale without anyone noticing.

Multi-agent orchestration handles the separation between the internal analysis (what's actually going on here, what's the risk level, what do we know about this customer) and the customer-facing draft (what we say externally). These run as distinct agents with different system prompts and different output constraints, rather than asking a single prompt to do both jobs at once. Keeping them separate produces better outputs from each and makes it easier to tune one without breaking the other.

Getting this to work reliably required careful prompt engineering, a well-structured indexing strategy, and iterative testing with the support team on real tickets. The editable organisational context document was important here too: giving the team ownership of what the AI knows means the system stays accurate as the product and processes evolve, without needing engineering intervention every time something changes.

My role throughout has been half product owner, half engineer: I decide what gets built, I build most of it, and I work with the team when I hit the edges of what vibe coding can do alone.

## How long it took

The first functional version took about a week. The project is ongoing — each phase adds meaningful capability. Phase 1 (live now) covers the full ticket-to-response workflow. Future phases will deepen the AI agent capabilities and expand the integration surface.

## Where it stands now

Phase 1 is complete. The dashboard works end-to-end: tickets appear in real time via webhook, customer context enriches on every ticket open, and the AI panel generates its analysis and draft automatically. The team is actively using it.

This is the most technically complex thing I've built so far, and it's the one I'm most proud of. It's also the clearest demonstration of what happens when you combine a deep understanding of the problem (I sat with the support team and watched how they worked) with the ability to move fast in code.
