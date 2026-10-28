# AmpleMarket Team Hub

> A shared workspace for building out the amazee.io outreach operation on AmpleMarket — consolidating research, strategy, and workflow documentation as the team gets up to speed on a new tool.

| | |
|---|---|
| **Built for** | amazee.io |
| **Timeline** | Ongoing |
| **Status** | Work in progress |
| **Team** | Solo for now; intended to become a shared team resource |
| **Stack** | Claude, AmpleMarket MCP server, Markdown; n8n and the HubSpot API for the reply pipeline |

---

## What this is

AmpleMarket is amazee.io's outreach and sales engagement platform — a new addition to the growth tech stack. As the owner of this tool, I'm responsible for building out its workflows, integrations, and processes, and eventually for enabling the wider team to use it effectively.

This repository is the working layer beneath that: research notes, ICP analysis, outreach strategy, sequence logic, and findings from the rollout so far. It's the documentation that makes the tool make sense to someone picking it up for the first time.

It's also where I'm developing my thinking about how AmpleMarket fits into the broader growth stack alongside HubSpot, Notion, and n8n — and where the integration points and automation opportunities are.

## What I'm exploring

A few threads running through this work:

**MCP server integration** — AmpleMarket has an MCP (Model Context Protocol) server, which means AI agents can interact with it directly. I'm using this to build Claude-powered workflows that can query contact data, suggest sequences, and surface signals from the outreach pipeline without requiring a human to manually pull reports.

**Team enablement** — eventually this hub will be the starting point for any team member working with AmpleMarket. Right now it's a solo research project; the goal is to make it legible and useful to others.

**Workflow and sequence design** — outreach that works requires more than a tool. It requires knowing who to reach, what to say, when to send it, and how to follow up. This is where I'm developing and testing that thinking.

## What I've built so far

**Claude working AmpleMarket directly.** Through the MCP server, Claude reads my lead lists and enriches leads without me exporting anything. In one test run it worked through a lead list of boutique marketing agencies:

- researched each lead and wrote a one-line personalised opener
- identified the three pain points that profile most likely shares, such as running client infrastructure without a DevOps team
- searched AmpleMarket for five lookalike companies that weren't in any existing list, verified each one through AmpleMarket's own enrichment, and ranked them by who to approach first and why

The prompts are kept in the repo so the run can be repeated on any list.

**Replies flow into HubSpot.** When a prospect replies to an AmpleMarket sequence, an n8n workflow I built picks up the webhook. It keeps only genuine, actionable replies (interested, asked to circle back, forwarded to the right person) and updates the HubSpot contact with the sequence and the kind of reply. Sales sees how someone replied, not just that they did. That pipeline is described in [RevOps Automation](./amazeeio-revops-automation.md), and [Canary](./canary/canary.md) checks it each weekday.

## Where it stands now

Early stage. This is research-in-progress rather than a finished product. I'm including it in this portfolio because it represents a different kind of work: not building an app, but thinking carefully about how a new tool fits into an existing operation and what it would take to use it well at scale.

The transition from "I'm the only one who knows how this works" to "the team can use this independently" is a project in itself — and it's one I've done before (Confluence to Notion, HubSpot from scratch). This is the next version of that work.
