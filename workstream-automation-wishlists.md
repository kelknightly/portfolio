# Workstream Automations

> How I work with each team at amazee.io to turn "we should automate that" into a shared, sorted backlog — then choose the right tool for each item, from HubSpot's built-in features to AI agents, and build the ones that pay off. Shown here with Marketing, BizOps and HR.

| | |
|---|---|
| **Built for** | amazee.io workstreams (Marketing, BizOps and HR shown here) |
| **Timeline** | Ongoing |
| **Status** | Live process; items move from idea to built |
| **Team** | Me, with each workstream's lead and team |
| **Stack** | Notion (wishlist databases, dashboards), Notion API, HubSpot and its Marketing Events API, BambooHR API, Xero, Google Sheets, GitHub, OpenClaw agents, n8n, Notion AI, Zoom |

---

## The problem

Every team has a list of repetitive work they'd love to hand off, but it usually lives in people's heads or comes up as one-off requests in Slack. That makes it hard to see which jobs would save the most time, which ones a tool we already pay for can do, and which actually need something built. Without a shared list, the loudest request gets done first, not the most useful one.

## How it works

For each workstream I set up an automation wishlist: a Notion database the team and I own together. Every item is a concrete task, not a vague wish. "Pause low-performing ads" is useful; "use AI for ads" isn't. Each item has:

- **Category**, so we can see which part of the team's work is most manual
- **Status**: To Discuss, To Start, In Progress, Ready
- **Tool**: what it should run on
- **Owner** and **Priority**, filled in once the team has talked it through

The team adds items whenever they notice repetitive work. We review the list together, and I decide with them which tool fits each item. That choice is the part of the job that matters most. Many items don't need anything new: HubSpot already handles welcome sequences, A/B testing and lead magnet delivery, so those get configured, not built. Items that need judgement or work across several tools go to an agent. Items that move data between systems on a schedule go to n8n or a script.

## Marketing: the wishlist

Marketing's list holds 23 tasks across six categories:

| Category | Tasks | Examples |
|---|---|---|
| Email | 6 | Writing email content, welcome sequences, A/B tests, delivering lead magnets, a bot that pulls email performance metrics |
| Content & SEO | 5 | Keyword ranking shifts, broken link checks, blog outlines, transcribing video and audio, a content to-do list from meeting transcripts |
| Social media | 4 | Creating and scheduling content across platforms, auto-replies to common DMs, brand and competitor mentions |
| Paid ads | 4 | Adjusting bids, pausing weak ads, lookalike audiences from HubSpot, budget alerts |
| Reporting | 3 | One central dashboard, weekly performance summaries, competitor tracking |
| Contacts & leads | 1 | A daily update on new contacts and contacts that became leads |

At the time of this snapshot, 2 items were ready, 5 were in progress, 4 were waiting to start, and 12 were still being discussed. Nine are assigned to OpenClaw agents, three to HubSpot's own features, and one to Notion and Zoom.

Some of these became full projects:

- **"Pulling data into a central dashboard"** became the [Mammoth Marketing Dashboard](./mammoth-marketing-dashboard.md): traffic, conversions, ad spend and pipeline in one place, refreshed daily.
- **"Create a content to-do list from meeting transcripts"** became [Customer Conversation Insights](./customer-conversation-insights.md): Notion AI turns recorded customer meetings into content briefs and talking points.
- **Content creation and email performance metrics** are ready to run as OpenClaw agents.

## Marketing: events from Notion to HubSpot

Marketing plans its events in a Notion database, but HubSpot needs the same events as Marketing Events, so that contacts and results can be attached to them. [Badge scanner contacts](./amazeeio-revops-automation.md#event-badge-scanner-contacts-badge-scanner--hubspot), for example, are imported against the HubSpot event. Without a sync, every event had to be entered twice.

I built a sync from Marketing's *Event Database* in Notion. When Marketing ticks **Sync to HubSpot** on an event, a daily job creates the event in HubSpot as a Marketing Event, with the same name, dates, category, description and link. It then writes the new ID into the event's **HubSpot Event ID** field in Notion.

That write-back is the part the team notices. The ID in the Notion row confirms the sync worked, so nobody has to open HubSpot to check, and an empty ID field shows straight away which events haven't synced. And because nothing syncs until the box is ticked, Marketing decides when an event goes into the CRM.

A few details made it reliable:

- **Safe to run again.** Each HubSpot event is keyed to its Notion page, so running the job twice never creates duplicates. HubSpot's Marketing Events API rejects a second create with a conflict error that includes the existing event's ID. The script treats that as "already synced" and fills in the Notion field if it's still empty.
- **Working within the API.** With the app token the sync uses, HubSpot's Marketing Events API allows creating and listing events, but not editing or deleting them. So the script only ever creates. To push a renamed event, you delete it in HubSpot and the next run recreates it. Unticking the box stops further syncing but never deletes anything in HubSpot.
- **Sensible times.** Marketing records dates, not times, so the script sets default working hours based on each event's region. HubSpot gets a real time window instead of midnight.
- **Watched.** It's a TypeScript script in the [Sync Hub](./marketing-sync-hub.md) codebase, run once a day by a scheduled job on the hosting platform. [Canary](./canary/canary.md) checks every day for events that have been ticked for more than 25 hours with no HubSpot ID, which means the sync has failed or hasn't run.

## BizOps: finance data from four Xero entities

BizOps already tracked its own automation work and counted the time saved: about 3 hours a month from its first small fixes. I worked with the team to find the next, bigger wins.

We mapped the manual processes first. Most followed the same pattern. amazee.io runs four Xero entities in different countries, and many BizOps spreadsheets needed data from all four. Someone had to log in to each entity, download a report, and import it into a Google Sheet:

- revenue, pulled 3–4 times during each month-end close for preliminary and final results
- expenses, tax data and employee allowance tracking, on their own schedules
- a weekly Jira report, downloaded as CSV, reworked in a Google Sheet, then imported into BambooHR. Automated, it could run daily instead. This one is still manual and next in line.

Because so many sheets depended on that one pattern, we picked "four Xero entities into one Google Sheet" as the first automation. It was simple enough to start with, and solving it once would open up the expense, tax and allowance sheets after it.

Before building anything, we compared the options:

- **An existing consolidation tool.** BizOps was already testing JOIN, which combines all four Xero entities with a daily sync. Its dashboards were useful, but in testing its AI features weren't reliable, and it couldn't give the access the spreadsheets needed.
- **Buy a connector.** A Xero-to-Google-Sheets connector cost about $30 a month.
- **Build it.** A custom script meant roughly 20 hours of work, mostly on Xero's OAuth 2 sign-in and token refresh, to save 3+ hours a month.

On those numbers, buying the connector came out ahead, but we agreed to test both, because four entities is where off-the-shelf connectors tend to struggle. For the build route, I sketched two designs: one sheet per entity feeding a master sheet, or a local script that pulls and combines all four before writing to Sheets.

BizOps went with the script, after testing both routes.

I also set the team up to own its scripts, not depend on me. Scripts go in a shared GitHub repository instead of Google Apps Script, so they have version control and review. API keys stay in a local environment file on each person's machine and never go into the repo. Anyone on the team can clone it and run a script with their own keys. One of the team's bookkeepers wanted to learn the coding side, so I helped set up the repository as somewhere to practise, with Claude in VS Code as the recommended way to write scripts.

## HR: BambooHR to a private Notion dashboard

HR's data lives in BambooHR. HR wanted recurring reports from it, such as headcount by department or the make-up of each team, without exporting and rebuilding the tables by hand.

I moved that reporting into a structured, private Notion dashboard in the HR teamspace, filled from the BambooHR API through the Notion API. It's running now, run on demand; a schedule comes next.

This one is also an enablement project. The Head of HR wants to be able to run and change it himself, so we meet for a weekly workshop and go through it piece by piece: where the data comes from, how a report is built, and why each of the safeguards below exists. The aim is a dashboard HR owns, not one that depends on me.

HR data needed more care than most:

- **Privacy first.** The dashboard lives in a private HR teamspace, and it holds aggregate reports, not copies of individual employee records. Keeping BambooHR data confidential was listed as a requirement from the start, not as an afterthought.
- **Don't trust the filter.** BambooHR's "current employees only" option didn't actually filter: it returned past employees as well. Every report asks for employment status and filters in code, so headcounts are correct.
- **Make totals add up.** Where a field is empty for some employees, they're counted as "Not specified" instead of being dropped, so every table still adds up to the real headcount.
- **Say how old the data is.** Each report carries a line saying where the data came from, the headcount it covers, and the date it was pulled. These are snapshots, and readers need to know how current one is.
- **Writes are checked.** Notion confirms a write when it receives it, not once the content is in place. So after each write the page is read back to confirm the report is there, and a report is never appended again without checking first, which would duplicate it.

## How I approached it

The wishlist is as much a conversation tool as a backlog. Writing each item as a specific task forces the team to say exactly what they want, and that often shows the job is smaller than it sounded, or that a tool we already have can do it.

I keep "To Discuss" as a real status. Twelve of Marketing's 23 items sit there because automating them needs a decision first. For example, should a bot be allowed to pause ads or adjust bids on its own, or only suggest changes? Those are questions for the team, not technical ones, and the list makes them visible.

Across the builds, the same rule holds: people should be able to see that an automation worked from where they already work. The HubSpot event ID landing in Notion, and the date line on each HR report, do that job. An automation the team has to double-check by hand doesn't save them much.

And building isn't always the answer. With BizOps, the numbers showed that a $30-a-month connector might beat 20 hours of custom work. Saying so is part of the job, even when building is the more interesting option.

## Where it stands now

The process runs with Marketing, BizOps and HR. BizOps and HR are both learning to build and run their own automations, and the same format works for any workstream. It also connects to the [agentic engineering workshops](./README.md#how-i-work) I run, where workstream leads learn to build some of these automations themselves instead of waiting for me.
