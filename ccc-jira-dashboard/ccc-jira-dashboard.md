# Customer Command Centre (CCC)

> A real-time operational dashboard that maps every Jira support ticket to a customer, so Customer Engagement Managers always know what's on fire — without opening Jira.

| | |
|---|---|
| **Built for** | amazee.io |
| **Timeline** | 3 days |
| **Status** | Superseded — absorbed into Support Relay |
| **Team** | Solo |
| **Stack** | Next.js, TypeScript, PostgreSQL, Jira API, Notion API |

---

## The problem

Customer Engagement Managers at amazee.io were responsible for knowing the support health of every customer they managed. In practice, that meant keeping Jira open in a browser tab and mentally tracking which customers had tickets, how long those tickets had been open, and which ones were escalating. There was no single view. There was no alert system. If you missed a ticket, you missed it.

The Red / Amber / Green (RAG) concept existed in spreadsheets and conversation, but nowhere that updated automatically.

## What I built

CCC is an always-on dashboard that pulls live data from Jira Service Management and maps each ticket to the customer it belongs to, enriched with context from Notion. Every customer shows a RAG status based on ticket age, volume, and priority. CEMs can see their entire portfolio at a glance — who's green, who's at risk, who needs a call today.

The dashboard updates in near real-time. No manual refresh, no digging through queues, no cross-referencing spreadsheets.

## How I built it

I built this entirely through vibe coding — describing the problem to Claude in VS Code and iterating on the output until it worked the way the team needed it to. The main technical challenge was the Jira-to-customer mapping: Jira doesn't natively know which customer an organisation belongs to in the same way Notion does, so I built a sync layer that bridges the two.

The stack is Next.js with TypeScript, a PostgreSQL database for storing snapshots and customer mappings, and direct API integrations with both Jira and Notion.

## How long it took

Three days from blank canvas to a working dashboard the team could use. That's not unusual for projects like this when the problem is clearly defined and the data sources are accessible — most of the time was spent on the Jira-Notion mapping logic and getting the RAG thresholds right.

## Where it stands now

CCC was eventually superseded by Support Relay — a more ambitious project that absorbed the CCC's ticket-mapping and RAG-scoring logic and combined it with AI assistance, direct JSM reply capabilities, and richer customer context. The CCC's sentiment data is still being consumed by Support Relay as a live signal layer.

The fact that CCC was absorbed rather than retired is, in a way, the best outcome: it proved the concept well enough that it became a core dependency of the next tool.
