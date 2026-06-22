# User Adoption Tracker

> A lightweight Python script that pulls weekly engagement metrics from the internal agentic apps and writes them to a Notion database — closing the loop between building tools for the team and knowing whether those tools are actually being used.

| | |
|---|---|
| **Built for** | amazee.io |
| **Timeline** | A few days |
| **Status** | Live |
| **Team** | Solo |
| **Stack** | Python, Notion API, internal Growth Dashboard API |

---

## The problem

Once you've built several internal tools and people are using them daily, a new question emerges: how much? And by whom? And is that changing week over week?

At amazee.io I'd built and deployed three tools — the HubSpot Growth Dashboard, the HubSpot Sales Triage Tool, and the Marketing/BizDev/Sales Sync Hub — and they were in regular use. But I had no structured way to track engagement over time. Usage data existed in the Growth Dashboard's backend, but nothing was surfacing it in a place where the team could see it passively, and no one was looking at trends.

The informal answer to "is this being used?" was "I think so" or "I heard someone mention it." That's not good enough if you're trying to understand whether a tool is earning its keep or quietly going stale.

## What I built

The User Adoption Tracker is a Python script that runs weekly and computes per-app metrics — DAU, WAU, MAU, total events, and a week-on-week trend — for each of the three internal tools. It fetches the data from an internal API endpoint on the Growth Dashboard (which already had the event tracking built in), formats it into rows, and upserts them into a Notion database called "User Adoption of Agentic Apps."

Each app gets a persistent row in Notion that's updated in place every week, with a full metrics table in the page body. The script also identifies the top user action per app for the period, which is useful for understanding what people actually do in each tool rather than just whether they showed up.

It can be run with `--dry-run` to print the metrics to the terminal without writing to Notion, and `--week-of` to backfill a specific week if needed.

## How I built it

The Growth Dashboard already exposed an internal API for adoption metrics — I'd added that endpoint earlier when building out the dashboard itself. This script is the consumer of that API, plus the Notion writer to make the data persistent and visible without anyone having to actively check.

The whole thing came together quickly because the data pipeline already existed. The main work was the Notion upsert logic — deciding on a schema that would be useful to read week over week, and writing the code to update existing rows rather than append duplicates every time the script runs.

It's a small piece of work, but it completes a loop that I think matters: you should be able to answer "are people using this?" for any tool you ship.

## Where it stands now

Live and running. The Notion database gives an at-a-glance view of weekly adoption trends across all three tools, and it's straightforward to extend when a new tool gets added to the tracked set.
