# HubSpot Growth Dashboard

> A live visibility tool for the amazee.io Growth team — tracking how HubSpot Deals and Leads are changing over days, weeks, months, and years, without anyone having to build a report manually.

| | |
|---|---|
| **Built for** | amazee.io |
| **Timeline** | 1 week |
| **Status** | Live |
| **Team** | Solo |
| **Stack** | Next.js, TypeScript, Supabase, Prisma, PostgreSQL, Docker, TailwindCSS |

---

## The problem

HubSpot has reports. What it doesn't have — at least not without significant configuration effort — is an easy way to see change over time across deals and leads in the way a growth team actually needs to think about the pipeline. How many new leads came in this week versus last week? Which deals moved stages? What does the shape of the pipeline look like right now compared to a month ago?

The Growth team was piecing this together from HubSpot's native views and occasional manual exports. It worked, but it was slow, it wasn't consistent, and it meant nobody had a shared, live picture of what was actually happening.

## What I built

The HubSpot Growth Dashboard is a purpose-built visibility layer over HubSpot data. It pulls deal and lead data via the HubSpot API, stores snapshots in a PostgreSQL database via Supabase, and presents the changes over configurable time windows — day, week, month, year — in a clean, browsable interface.

The key thing it provides is persistence: because it takes daily snapshots, you can actually compare where you are now to where you were. HubSpot itself doesn't easily give you that without building custom reports. This does.

## How I built it

Built solo using my standard vibe coding approach — Claude and VS Code, iterating quickly on the data model and UI. The stack (Next.js, Supabase, Prisma) is one I'd settled into by this point and could move fast with. The main complexity was designing the snapshot mechanism to be both reliable and queryable in the ways the team needed.

The snapshot script runs on a schedule and stores a structured record of deal and lead state each day. The frontend queries across those snapshots to calculate deltas and present them cleanly.

## How long it took

One week from start to a deployed, live dashboard the team was using. Documentation, setup guides, and environment configuration were all built alongside the tool itself — not bolted on afterwards.

## Where it stands now

Live and in active use by the Growth team. It's one of the more straightforward tools in this collection — the problem was clearly defined, the data was accessible, and the output was something the team immediately understood and trusted.

That reliability is part of the value. A dashboard that's accurate and always on is worth more than a sophisticated one that nobody checks.
