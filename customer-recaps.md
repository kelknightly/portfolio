# Customer Slack Pulse Agent

> An AI agent that reads a week of Slack activity across all customer channels, analyzes it with an LLM, and writes structured pulse reports to Notion — giving the Sales team a single weekly digest of every customer relationship without anyone having to manually review dozens of channels.

| | |
|---|---|
| **Built for** | amazee.io |
| **Timeline** | A few days |
| **Status** | Live |
| **Team** | Solo |
| **Stack** | Python, Slack SDK, Notion API, AMAZEE AI (Claude), n8n |

---

## The problem

amazee.io maintains shared Slack channels with its customers — a common setup for managed hosting and professional services companies where support, escalations, and ongoing conversations happen in the open. The Sales team, and specifically the account manager, was supposed to stay on top of all of these channels to catch churn signals, active incidents, billing conversations, or anything else that might affect a deal or a renewal.

In practice, keeping up with dozens of customer channels manually doesn't happen. It's too much ambient noise, and there's no structure to it — you'd need to check each channel, decide what matters, and somehow synthesise it into something actionable. That work was either getting skipped, or falling entirely on one person to manage as a daily burden.

## What I built

The Customer Slack Pulse (CSP) Agent is a Python script that runs weekly and does the following automatically:

1. Reads the Notion Company Database to find all companies that have a Slack channel URL configured.
2. Fetches the past seven days of messages from each channel via the Slack SDK, including thread replies, with user mentions resolved to display names.
3. Passes the message history to AMAZEE AI (Claude, via the amazee.ai internal API) with a structured prompt that asks for two things: a list of noteworthy Pulse Items (incidents, escalations, churn signals, migration discussions, billing issues — anything worth flagging), and a Weekly Report summarising the week for that company.
4. Writes the resulting items to a Notion "Customer Slack Pulse" database, one row per pulse item and one row per weekly report.
5. An existing n8n workflow picks up new CSP rows via webhook and syncs the summaries to the relevant HubSpot deals automatically.

The LLM is instructed to produce JSON only — no markdown fences, no prose — so the output can be parsed reliably and written directly to Notion properties. Severity is categorised as Critical, Important, or FYI. Weekly reports are always generated even when there's nothing notable to flag, so the absence of noise is also surfaced.

## How I built it

Built solo with Claude in VS Code. The main components were the Slack message ingestion (including thread inlining and user resolution), the Notion company database reader (which does dynamic field discovery rather than hardcoding property names, so it's not fragile to Notion schema changes), and the LLM analysis layer with the structured prompt.

The n8n handoff to HubSpot was already in place from earlier RevOps work — this script just needed to write to the right Notion database in the right schema for the workflow to pick it up. That meant I got HubSpot integration essentially for free by designing the output to fit the existing pipeline.

The trickiest part was prompt design: getting the LLM to produce consistent, well-structured JSON across a wide variety of Slack conversations — ranging from near-silent channels to incident threads with dozens of messages — without hallucinating structure or producing inconsistent severity ratings. The "🛰 Customer Slack Pulse:" prefix convention on every summary line was a deliberate forcing function to make the output scannable in HubSpot activity feeds.

## Where it stands now

Live and running for the Sales team. The account manager gets a weekly Notion digest and HubSpot notes without having to monitor any channels directly. Critical-severity items can be caught same-week rather than discovered late in a renewal conversation.

This is a good example of agentic work that isn't flashy but is genuinely useful: it removes a real cognitive burden, runs reliably without supervision, and slots into an existing data pipeline rather than creating a new one.
