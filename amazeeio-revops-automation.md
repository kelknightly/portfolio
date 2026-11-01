# amazee.io RevOps Automation

> The data infrastructure behind amazee.io's sales pipeline — a collection of n8n workflows, scripts, and AI-assisted pipelines that automatically move lead and contact data from wherever it originates into HubSpot, enriched, attributed, and ready for outreach.

| | |
|---|---|
| **Built for** | amazee.io |
| **Ongoing since** | 2024 |
| **Status** | Live and actively maintained |
| **Team** | Solo build (the trial export Action built with the Head of Product); operates across Sales, Marketing, and RevOps functions |
| **Stack** | n8n, HubSpot, Node.js, TypeScript, GitHub Actions, LiteLLM, Polydock webhooks, Redis, Notion, Google Sheets, Slack |

---

## The problem

A sales and RevOps function only works if the CRM is accurate. At amazee.io, lead data was coming in from multiple sources — product signups, outbound sequences, event badge scanners, partner programs, AI trial dashboards — and none of them talked to HubSpot automatically. The result was a mix of manual CSV uploads, missed contacts, and incomplete attribution. Nobody had a clean picture of where leads were coming from or how engaged they were.

The work described here is the infrastructure I built and maintain to fix that: a set of automated pipelines that ingest contacts from each source, apply the right attribution and marketing compliance properties, and keep HubSpot current without anyone needing to intervene.

## What I built

### amazeeClaw signups (Polydock → HubSpot)

amazeeClaw is a product offered through Polydock at `my.amazee.io/amazeeclaw`. When a user completes a signup, a webhook fires an event to an n8n workflow I built. The workflow filters specifically for `app_instance.status_changed` events where the claim is complete and a user email exists, then upserts the contact into HubSpot — creating or updating without duplicating — and sets lead source attribution to "Inbound - amazeeClaw Signup".

A separate branch handles marketing opt-in: if the user checked the product updates box during signup (`opt_in_to_product_updates = 1`), the workflow makes a PATCH request to HubSpot to set both `amazeeclaw__opted_in_to_marketing` and `hs_marketable_status` to true. If they didn't, those properties are left alone. Once in HubSpot, these contacts are automatically enrolled into a three-email sequence irrespective of their marketing contact status.

### AmpleMarket outbound replies (AmpleMarket → HubSpot)

amazee.io runs outbound sequences through AmpleMarket. When a prospect replies, AmpleMarket sends a webhook. The workflow only processes replies that are both genuine replies (`is_reply = true`) and carry actionable labels — Interested, Introduction, Asked to circle back later, or Forwarded to the right person. Anything labelled Not the right person or other negative sentiments terminates at the filter node without touching HubSpot.

For contacts that pass the filter, the workflow upserts the contact and then PATCHes the HubSpot record with the sequence name, a response flag, and the specific label type — so the sales team can see not just that someone replied, but how they replied and which campaign they were in.

### Drupal AI trial users (GitHub Action + n8n → HubSpot)

amazee.ai offers a free Drupal AI trial: a Drupal site gets an AI key and 30 days of usage. The trial data lives in the AI gateway, not in a CRM, so sales had no view of who was trying it or how heavily.

A GitHub Action, built with the Head of Product, runs three times a week (Sunday, Tuesday and Friday at 6:00 AM UTC). It queries all six LiteLLM regions, keeps only Drupal AI trial keys (identified by the site hostname in the key alias, with test and anonymous trials excluded), and adds up each team's spend and usage. It then checks each team against the amazee.ai backend to get its status: paying customer, active trial, expired trial, registered user or dev contributor. The result is sorted by spend and posted as a CSV to an n8n webhook, and a dated copy is committed to the repository as a history. By mid-2026 each run carried about 325 trial accounts.

The n8n workflow reads the CSV and loops through it in batches, with a short wait between calls to stay inside HubSpot's rate limits. For each person it creates or updates the contact, then sets a group of properties I created for AI trials:

- attribution: lead source `Inbound - AI Trial Sign Up`, acquisition subtype `AI Trials`, and an "engaged with trials" flag
- status and engagement: customer status, total spend, days in trial, trial days remaining
- consent: whether the contact can be sent marketing, copied from HubSpot at the time of import
- freshness: the date the record was last updated from the trial data

When the run finishes, the workflow posts a one-line summary to the project's Slack channel: how many contacts were imported, broken down by region.

Because the properties are kept up to date on every run, the data can be used for segmentation. Sales can filter new contacts by `Inbound Acquisition Subtype: AI Trials`, find everyone who has ever tried it through `AI Trial - Engaged with Trials: Yes`, and sort by spend or days remaining to see who to call first.

### Drupal.org AI Demo contacts (Polydock + Google Sheets + n8n → HubSpot)

Drupal.org offers AI demo sites, run on amazee.io's Polydock platform. This pipeline has two n8n workflows, with a person deciding what goes into HubSpot in between.

**Registrations, as they happen.** Polydock sends a webhook for every demo event. When a registration changes status, the first workflow adds or updates the person in the *Drupal.org AI Demo Leads* Google Sheet and posts a formatted card to Slack with their name, registration ID and the demo app. When a new demo site starts being set up, the workflow opens a Slack thread for it, and posts each later status change as a reply. Redis stores which thread belongs to which site, so every update for one demo appears in one thread instead of spread across the channel.

**Claiming, then importing.** Not every demo user is amazee.io's lead, so nothing goes to HubSpot automatically. A team member finds a record amazee.io has claimed and sets the `Claimed by amazee.ai` column to "Yes". Every few minutes, the second workflow picks up rows marked "Yes", up to ten at a time. It creates or updates each contact in HubSpot with their name, sets `Signed up for demo from Drupal.org` to true, and marks the row "Yes - Imported" so it's never processed twice.

After the contact lands in HubSpot, a companion HubSpot workflow takes over. It sets marketing opt-in to No by default, because these people signed up on Drupal.org and aren't expecting general amazee.io marketing. For any contact without existing attribution, it fills in the acquisition type, subtype and source.

### Event badge scanner contacts (Badge Scanner → HubSpot)

For events like DrupalCon, attendee data is captured with a badge scanner tool and exported as a CSV. I wrote and maintain the standard operating procedure for getting this data into HubSpot cleanly — including the critical file naming convention (`DD-Mon-YYYY [Event Name] [City] - Contacts - Badge Scanner`) that triggers HubSpot's automation to set the correct acquisition source from the filename alone.

The process imports contacts directly against the relevant Marketing Event in HubSpot (not as a general contact import), and includes specific field exclusions for scanner metadata columns that shouldn't pollute the CRM. After import, a bulk edit step sets `Import: Trigger Create Lead` to Yes, which fires a HubSpot workflow that creates a Lead record and moves the contact to lifecycle stage `3 - In Outreach`.

### HubSpot → Notion reporting (Node.js scripts and Notion charts)

In the other direction: Sales and RevOps run their weekly reporting in Notion, so I built a set of Node.js scripts (February–March 2026) that bring the HubSpot numbers to them. They write to Notion databases, and Notion's own chart blocks turn those databases into the charts the team reviews each week.

- **Contacts by lifecycle stage.** The script counts every HubSpot contact by lifecycle stage and writes one row per stage. Only the totals reach Notion, never individual contact records, and a Notion bar chart reads the rows. The weekly version writes the current counts into the table's first number column, and the team keeps earlier weeks' columns alongside it in Notion, so the table holds a week-by-week history.
- **Leads by pipeline stage.** The Leads page in Notion holds one table per pipeline. The script matches each table to its HubSpot pipeline by name, pulls the count at every stage, and writes it into the current calendar week's column.
- **Lead movements in the last 7 days.** Using HubSpot's property history, the script finds every lead whose pipeline stage changed in the past week. It records the previous and current stage, whether the lead advanced or went backwards, and a link to the record. The table is rebuilt each run, so it always shows exactly the last seven days.
- **Deal movements in the last 7 days.** The same view for deals, built from HubSpot's "date entered stage" properties.
- **A weekly deal-changes report.** A markdown report of every deal's stage changes, notes and emails from the past week, with the HTML stripped out so it reads cleanly.
- **An active-customer sources audit.** A CSV that shows where each active customer's contact record originally came from, used to check attribution.

`npm run sync:all` runs the lead movements, pipeline counts and lifecycle counts in order each Monday, and stops if one fails rather than publishing a half-updated report. Each sync either updates existing rows or rebuilds its table from scratch, so re-running one never creates duplicates.

The repository also holds a HubSpot skill file for Claude. It teaches an agent how amazee.io's HubSpot is laid out before it fetches anything: how to tell from someone's wording whether they mean a lead, a contact or a deal (the most common mix-up), how to compare a deal's or lead's history with its current status, how to count contacts, and how to format reports for Slack. The repository is private.

For a live, no-code view of the same CRM, see [Marketing Pulse](./marketing-pulse.md), a Claude dashboard that queries HubSpot directly through its MCP server.

### Sales-to-marketing AI content pipeline (Zoom Revenue Accelerator + Notion AI)

A lighter-weight workflow I designed to extract signal from sales conversations for the marketing team. Sales reps download transcripts from Zoom Revenue Accelerator and add them to a Notion database along with meeting metadata. Notion AI then runs a fixed prompt on a schedule to populate two downstream databases: one that categorises and counts the topics and questions prospects raise most often, and one that drafts content briefs based on those patterns. An n8n workflow that takes transcripts straight from Zoom's recording webhook into Notion is built and tested, waiting to be connected to amazee.io's Zoom account. The full write-up is in [Customer Conversation Insights](./customer-conversation-insights.md).

### Marketing events (Notion → HubSpot)

Marketing plans events in a Notion *Event Database*. When they tick **Sync to HubSpot** on an event, a daily scheduled TypeScript job creates the matching Marketing Event in HubSpot with the same details and writes the ID into the event's **HubSpot Event ID** field, so the team can see it synced without opening HubSpot. The job is safe to re-run (each event is keyed to its Notion page), and [Canary](./canary/canary.md) flags any event still waiting for an ID after 25 hours. More in [Workstream Automations](./workstream-automation-wishlists.md#marketing-events-from-notion-to-hubspot).

## How I approach this work

Each of these pipelines started from the same place: a manual process, a CSV upload, or a gap where data simply wasn't being captured at all. The job is to identify the gap, understand the compliance requirements around it (particularly marketing consent and attribution), build the automation, and then write the process documentation so that anyone on the team can understand what's happening and operate it without me.

The compliance piece matters more than it might seem. Getting a contact into HubSpot is straightforward. Getting them in with the right marketing status, the right attribution, the right lead source — and making sure that automated workflows downstream don't fire incorrectly — requires thinking through the full lifecycle of that record, not just the moment of ingestion.

## Where it stands now

All of these pipelines are live and operational. The badge scanner process and the Drupal.org Demo workflow are team-facing — other people follow the processes to operate them. The n8n workflows run autonomously. The HubSpot → Notion sync scripts run on a schedule or are triggered manually for weekly reporting.

This is infrastructure work. It doesn't have a launch date or a demo video. What it has is a sales team that can trust their CRM data and a RevOps function that isn't spending time on manual data entry.
