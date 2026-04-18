# Drupal AI Trials Lead Gen

> A daily automated pipeline that identifies which Drupal AI trial users are most engaged, scores them by spend and activity, and routes the data to the RevOps team — turning a manual, irregular process into something that just runs.

| | |
|---|---|
| **Built for** | amazee.io |
| **Timeline** | 1 day |
| **Status** | Live |
| **Team** | Collaborative — me, the engineering team, and the amazee.ai team |
| **Stack** | Python, LiteLLM API, amazee.ai backend API, n8n, CSV export |

---

## The problem

amazee.io offers Drupal AI trials across multiple LiteLLM regions — DE, US, AU, CA, CH, UK. Trial users who engage meaningfully (spending real compute budget, returning multiple times, using multiple features) are strong conversion candidates. Trial users who signed up and never came back are not.

The problem was that nobody had a reliable, current view of which trial users were in which category. Identifying high-engagement trials required querying multiple regional APIs, cross-referencing against the amazee.ai backend to check customer status, and compiling the results somewhere useful. It was either not being done, or being done manually and inconsistently.

## What I built

An automated daily export pipeline that does all of this without human involvement. Every day, the script queries all six LiteLLM regions, aggregates spend and usage data per team, filters out anonymous trials, checks each team's status against the amazee.ai backend API, and exports the result as a CSV sorted by highest engagement first.

That CSV is then posted via webhook to n8n, where it flows into the RevOps processing workflow. The Sales team gets a prioritised, current list of trial users every morning — ranked by the signal that actually matters for conversion.

## How I built it

This was a collaborative effort — I didn't build in isolation. The engineering team provided guidance on the LiteLLM API surface and regional query patterns, and the amazee.ai team helped with the backend API integration for customer status lookups. My role was designing the overall pipeline, writing the aggregation and filtering logic, and wiring the n8n webhook at the output end.

The script itself is Python. No web server, no database, no UI — just a well-structured script that runs on a schedule, does its job, and produces a clean output. Sometimes that's exactly the right tool for the problem.

## How long it took

One day. This is what happens when the problem is well-defined, the data sources are documented, and you have the right people in the room. Most of the day was spent on the cross-region aggregation logic and the customer status enrichment — the webhook and CSV export were the easiest parts.

## Where it stands now

Live and running daily. The RevOps team gets their prioritised trial list automatically. This is one of those projects where the output is more impressive than the tool itself: a small, focused piece of automation that removes a recurring manual task and ensures the right leads get attention at the right time.

It also demonstrates something worth saying explicitly: not every problem needs a dashboard or a UI. Sometimes the best solution is a script that runs quietly in the background and delivers exactly what's needed to exactly the right place.
