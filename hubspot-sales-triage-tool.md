# HubSpot Sales Triage Tool

> A triage workspace for the amazee.io Growth and Outreach teams to evaluate incoming contacts before they officially enter the HubSpot pipeline — the project that started everything.

| | |
|---|---|
| **Built for** | amazee.io |
| **Timeline** | A couple of months |
| **Status** | Live and in active use |
| **Team** | Mostly solo; an engineer helped with API connection improvements |
| **Stack** | Next.js, TypeScript, Tailwind CSS, shadcn/ui, HubSpot API |

---

## The problem

When contacts come in through various channels at amazee.io — forms, events, outreach responses, referrals — someone needs to evaluate them before they're formally converted into HubSpot Leads. Is this a good fit? Do we already have a relationship with this person or company? Is there enough context to pass them to Sales, or does this need more research first?

This process was happening informally, often in Slack threads and spreadsheets. There was no dedicated workspace for it, no consistent criteria, and no clear handoff mechanism. Good contacts were slipping through the cracks because there was no structure around the triage step.

## What I built

The HubSpot Sales Triage Tool is a dedicated workspace for the Growth and Outreach teams to work through incoming contacts systematically. It surfaces enriched contact data, shows existing HubSpot relationships and history, and gives the team a clear interface for making and recording triage decisions before anything enters the formal pipeline.

It's connected directly to HubSpot — contacts flow in from the CRM, decisions and notes flow back out. The goal was to make triage feel less like admin and more like focused, informed decision-making.

## How I built it

This was my first ever vibe-coded project, which means it was also my biggest learning experience. I'd never approached building software this way before — describing what I wanted to Claude in natural language and iterating on the output until it worked. Looking back, the approach was right even when the execution was rough around the edges.

I got the tool surprisingly far on my own. The UI, the data display logic, the triage workflow — most of that I built without external help. Where I eventually hit a wall was the HubSpot API connections: auth flows, rate limits, edge cases in the data model. That's when I brought in an engineer, who helped me strengthen those integrations and make the tool production-grade.

That experience changed how I think about collaboration in this kind of work. Knowing what to build solo and when to bring in specialist help is its own skill.

## How long it took

A couple of months. That's longer than my later projects, and intentionally so — this was a learning process as much as a build process. I was figuring out the vibe coding approach, the HubSpot data model, and what the team actually needed (which evolved as they started using early versions). Every week or two I'd ship something new, get feedback, and update it.

By the time it was live, I'd built a mental model of HubSpot's architecture that I couldn't have gotten any other way. That knowledge paid dividends on every subsequent project.

## Where it stands now

Live and actively used by the Growth and Outreach teams. It's embedded in the team's workflow for handling incoming contacts. This tool also made me the de facto HubSpot point of contact and tool owner at amazee.io — not because I set out to be, but because I'd built a deep enough understanding of the system by building with it.

If I were to rebuild it now it would take a fraction of the time. But the version that exists works, and the team trusts it.
