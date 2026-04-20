# amazee.io RevOps Automation

> The data infrastructure behind amazee.io's sales pipeline — a collection of n8n workflows, scripts, and AI-assisted pipelines that automatically move lead and contact data from wherever it originates into HubSpot, enriched, attributed, and ready for outreach.

| | |
|---|---|
| **Built for** | amazee.io |
| **Ongoing since** | 2024 |
| **Status** | Live and actively maintained |
| **Team** | Solo build; operates across Sales, Marketing, and RevOps functions |
| **Stack** | n8n, HubSpot, Node.js, GitHub Actions, Notion, Google Sheets, Slack |

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

amazee.ai offers a Drupal AI trial through its dashboard. Trial usage data is aggregated weekly from six LiteLLM regions by a GitHub Action managed by the Head of Product. Every Sunday at 6:00 AM UTC, that data is sent to an n8n workflow via webhook.

The workflow loops through each record, upserts contacts into HubSpot, and syncs a set of AI-trial-specific properties I created: `customer_status` (paying customer, active trial, expired trial, registered user, or dev contributor), `total_spend`, `trial_days_remaining`, `days_in_trial`, and `last_active_date`. A Wait node is included in the loop to respect HubSpot API rate limits during bulk processing. On completion, the workflow posts a summary report to the `#project-amaeeai-matthew` Slack channel.

The data is also structured for segmentation — new contacts who came through this pipeline can be filtered in HubSpot by `Inbound Acquisition Subtype: AI Trials`, and all contacts who have ever engaged with the trial can be filtered by `AI Trial - Engaged with Trials: Yes`.

### Drupal.org AI Demo contacts (Google Sheets + n8n → HubSpot)

Contacts from the Drupal.org AI Demo program are collected in a Google Sheet. The process for a team member is simple: find a record that amazee.io has claimed, set Column W (`Claimed by amazee.ai`) to "Yes", and the automation handles the rest. Every five minutes, an n8n workflow scans the sheet for any rows where that column is exactly "Yes", processes up to ten at a time, upserts them into HubSpot, and updates the row status to "Yes - Imported" once done.

After the contact lands in HubSpot, a companion HubSpot workflow kicks in: it sets the marketing opt-in to No by default (these contacts came from Drupal.org and aren't expecting general amazee.io marketing), and for any contact without existing attribution, sets the acquisition type, subtype, and source fields automatically.

### Event badge scanner contacts (Badge Scanner → HubSpot)

For events like DrupalCon, attendee data is captured with a badge scanner tool and exported as a CSV. I wrote and maintain the standard operating procedure for getting this data into HubSpot cleanly — including the critical file naming convention (`DD-Mon-YYYY [Event Name] [City] - Contacts - Badge Scanner`) that triggers HubSpot's automation to set the correct acquisition source from the filename alone.

The process imports contacts directly against the relevant Marketing Event in HubSpot (not as a general contact import), and includes specific field exclusions for scanner metadata columns that shouldn't pollute the CRM. After import, a bulk edit step sets `Import: Trigger Create Lead` to Yes, which fires a HubSpot workflow that creates a Lead record and moves the contact to lifecycle stage `3 - In Outreach`.

### HubSpot → Notion sync (Node.js scripts)

In the other direction: a set of Node.js scripts I wrote that sync HubSpot data into Notion for the weekly reporting and dashboard work the Sales and RevOps teams run there. Three scripts, each with a specific job:

- `sync-lead-movements.js` — runs weekly (each Monday), populates a Notion database with one row per lead that moved pipeline stage in the last seven days
- `leads-by-pipeline-stage.js` — reads the Leads Notion page, fetches current deal counts by pipeline stage from HubSpot, and writes them into the relevant Notion database columns
- `sync-weekly.js` — syncs contact lifecycle stage counts into the main Notion reporting database, used to power bar charts of contacts by stage

All three can be run together with `npm run sync:all`, or individually. The repo is at [github.com/kelknightly/hubspot-to-notion](https://github.com/kelknightly/hubspot-to-notion).

### Sales-to-marketing AI content pipeline (Zoom Revenue Accelerator + Notion AI)

A lighter-weight workflow I designed to extract signal from sales conversations for the marketing team. Sales reps download transcripts from Zoom Revenue Accelerator and add them to a Notion database along with meeting metadata. Notion AI is then prompted to read those transcripts and populate two downstream databases: one that categorises and counts the topics and questions prospects raise most often, and one that drafts high-level content briefs based on those patterns.

The transcript-to-insight synthesis is still being refined — the prompt is actively being polished — but the structural scaffolding is in place. Next step is automating the transcript download from Zoom into Notion directly, which is currently still a manual step.

## How I approach this work

Each of these pipelines started from the same place: a manual process, a CSV upload, or a gap where data simply wasn't being captured at all. The job is to identify the gap, understand the compliance requirements around it (particularly marketing consent and attribution), build the automation, and then write the process documentation so that anyone on the team can understand what's happening and operate it without me.

The compliance piece matters more than it might seem. Getting a contact into HubSpot is straightforward. Getting them in with the right marketing status, the right attribution, the right lead source — and making sure that automated workflows downstream don't fire incorrectly — requires thinking through the full lifecycle of that record, not just the moment of ingestion.

## Where it stands now

All of these pipelines are live and operational. The badge scanner process and the Drupal.org Demo workflow are team-facing — other people follow the processes to operate them. The n8n workflows run autonomously. The HubSpot → Notion sync scripts run on a schedule or are triggered manually for weekly reporting.

This is infrastructure work. It doesn't have a launch date or a demo video. What it has is a sales team that can trust their CRM data and a RevOps function that isn't spending time on manual data entry.
