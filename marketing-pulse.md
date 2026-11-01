# amazee.io Marketing Pulse

> A live marketing dashboard built in Claude on HubSpot's MCP server: new contacts, the lifecycle funnel, the deal pipeline, email performance and website traffic on one page, queried straight from HubSpot every time someone opens it. No ETL, no database, no hosting.

| | |
|---|---|
| **Built for** | amazee.io (Marketing and leadership) |
| **Timeline** | October 2026 |
| **Status** | Live; shared across the organisation |
| **Team** | Solo |
| **Stack** | Claude, Claude dashboard artifact, HubSpot MCP server (CRM search, marketing email analytics, content analytics, owners), d3 |

---

## The problem

Marketing's numbers live in HubSpot, but HubSpot's own reports are spread across separate screens for contacts, deals, email and website analytics. Getting a single view of "how is marketing doing right now" meant opening several reports and lining them up by hand.

I'd already built a full pipeline-and-database dashboard for this kind of question: the [Mammoth Marketing Dashboard](./mammoth-marketing-dashboard.md), which brings Matomo, GA4, Google Ads and HubSpot together. That's the right tool when you need several sources joined and kept as history. But for a quick, always-current view of what's in HubSpot, a full build is more than the job needs.

## What I built

A Claude dashboard with 17 live data sources, every one a query against HubSpot through its MCP server. There's no copy of the data anywhere: each time someone opens the page, it asks HubSpot for the current numbers, using that viewer's own HubSpot access.

The page has a date range switch (30, 90 or 180 days), a pipeline picker, and five sections:

- **New marketing contacts.** New contacts per week, broken down by original source and by lead status.
- **Lifecycle funnel.** Contacts at each stage of amazee.io's lifecycle, from Identified and Engaging through Ready for Outreach, In Outreach and In Deal, to Active Customer and Churned.
- **Deal pipeline.** Open deals and open deal value by stage for the selected pipeline, deals created, and deals won.
- **Marketing email.** Emails delivered per week, open and click rates, the six-month email health score, and unsubscribes and hard bounces.
- **Website and content.** Page views per week, form submissions against new contacts, and the top pages by views.

Every number on the page is tied to the query that produced it, so anyone can click through to see where a figure came from and how fresh it is.

## How I built it

I built it in a single session with Claude, connected to HubSpot through the [HubSpot MCP server](./README.md#mcp-servers-i-work-with). Each data source is a stored HubSpot MCP call, so the page needs no code of its own to fetch data and no credentials beyond each viewer's own connection.

Most of the design work was choosing the right question for each chart. Lifecycle stages are amazee.io's own custom stages, not HubSpot's defaults, so each one is a separate, named query. Pipelines are selectable because Sales runs several. Email health and website traffic come from HubSpot's analytics tools rather than CRM records.

## Where it stands now

Live and shared with the organisation. It doesn't replace the Mammoth dashboard; it complements it. Mammoth answers questions that need several sources and history. Marketing Pulse answers "what does HubSpot say right now?" in one place, and it was built in one session instead of weeks.

It's also an example of what I teach in [AI Enablement & Training](./ai-enablement.md): before building a pipeline and a database, check whether an MCP server and a live query will do.
