# Canary

> An internal workflow health monitor for amazee.io — a Python tool that reads workflow definitions from a Notion database, evaluates each one by making live API calls to HubSpot, Notion, n8n, and Amplemarket, writes results back, and sends Slack alerts when something breaks or recovers.

| | |
|---|---|
| **Built for** | amazee.io (internal tooling) |
| **Timeline** | April 2026 — 5 days |
| **Status** | Live |
| **Team** | Solo |
| **Stack** | Python, Notion API, HubSpot API, Amplemarket API, n8n API, Slack webhooks, GitHub Actions |

---

## The problem

As the RevOps automation work at amazee.io grew — more n8n workflows, more HubSpot automations, more Amplemarket sequences, more Notion-based processes — silent failures became a genuine risk. An automated workflow that stops producing results doesn't announce itself. Contacts stop flowing, data stops syncing, and nobody notices until someone manually checks a list that should have grown and hasn't.

The team had good documentation of what the workflows were supposed to do. What was missing was any systematic way to verify that they were actually doing it.

## What I built

Canary is a Python tool that treats a Notion database as the single source of truth for workflow health monitoring. Each row in the database represents one workflow: what platform it runs on, how often it should produce output, what that output looks like, and a pseudocode success criteria snippet describing what "healthy" means for that specific workflow.

When Canary runs, it reads those definitions, evaluates each one by making live API calls against the platforms involved, writes the result (Healthy / Unhealthy / Error) and a timestamp back to Notion, and fires Slack alerts into `#flow-canary` on status transitions. A workflow going from Healthy to Unhealthy sends one alert — not one per run. When it recovers, a recovery notice goes out. The alerting is deliberately transition-based rather than state-based, so the channel stays useful rather than becoming noise.

Scheduled runs happen via GitHub Actions on weekday mornings. The tool also supports manual invocation with flags for forcing all checks, targeting a workflow by name fragment, or doing a dry run that lists schedule status without executing anything.

## The interesting part: the DSL

The technically interesting piece is how Canary evaluates the success criteria. Each Notion row has a **Success Criteria** field containing a pseudocode snippet like:

```
count(contacts where lead_source_form=="Inbound - amazeeClaw Signup" and created_at>=now-24h) > 0
```

Canary includes a DSL parser that reads these snippets and translates them into real API calls — `count(contacts where ...)` becomes a HubSpot CRM search, `count(notion_rows in <db_name> where ...)` becomes a Notion database query, `count(n8n executions where ...)` queries the n8n execution history. Time expressions like `now-24h` and `now-7d` are resolved at runtime.

This architecture means the system is entirely data-driven. Adding a new workflow to monitor requires no code change — just a new row in the Notion database with the right fields. A non-engineer with access to Notion can define a new health check. That was a deliberate design choice: the tool should be maintainable by the people who know the workflows best, not just by whoever wrote the Python.

There's also a graceful degradation path: if a success criteria snippet references a HubSpot custom property that doesn't exist in the API, the check is marked Skipped (reported as Healthy) rather than producing a false Unhealthy. That prevents missing schema knowledge from generating noise.

## How the Slack alerting works

Three message types post to `#flow-canary`, each visually distinct:

- **Unhealthy / Error** — red or warning circle, fires when a workflow transitions *into* a broken state
- **Recovered** — green circle, fires when a workflow comes back to Healthy after being Unhealthy or Error
- **Manual reminder** — purple circle, a special case for workflows whose success criteria can't be automated; the snippet starts with `manual_reminder:` and Canary just pings the channel with the instructions

The transition logic is the important part. An alert fires only when `previous_status != "Unhealthy"` — so a workflow that's already broken doesn't spam the channel on every run. Recovery fires only when coming from Unhealthy or Error. The channel gets one message when something breaks and one message when it's fixed. That's it.

The Slack integration supports two delivery paths — an incoming webhook or a bot token — tried in that order depending on what's configured. In production via GitHub Actions, the bot token path is used, posting via Slack's `chat.postMessage` API.

## How long it took

Five days, from initial commit to GitHub Actions deployment. The scope was well-defined from the start: I knew exactly which platforms needed connectors, what the alerting behaviour should be, and what the Notion data model looked like. The DSL parser was the only genuinely novel engineering work — the connectors and alerting logic were straightforward once the data flow was clear.

## Where it stands now

Live and running weekday mornings. The Notion database now has a growing list of workflows with health status, last-checked timestamps, and last-result detail on every row. When something breaks, the team knows about it before they go looking.

Canary is also a small illustration of a broader principle in automation work: the workflows themselves are only as reliable as your ability to detect when they fail. Building the thing is one job. Knowing it's still working, three months later, is a different job — and it's one worth automating too.
