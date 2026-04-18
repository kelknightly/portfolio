# CEM Onboarding Tool

> An interactive prototype that makes the complexity of customer onboarding visible — so a team of six can have a meaningful conversation about it in the same room.

| | |
|---|---|
| **Built for** | amazee.io |
| **Timeline** | 3 hours |
| **Status** | Live as a decoupled prototype (no backend, no live data) |
| **Team** | Solo |
| **Stack** | Next.js, TypeScript, Tailwind CSS, nginx |

---

## The problem

Onboarding a new customer at amazee.io involves at least six teams: CEM, Platform Engineering, Solutions, Support, Sales, and BizOps. Each team has its own slice of the process. Nobody has a view of the whole thing.

The result is familiar to anyone who's worked in a fast-growing technical company: dependency blindness, SLA drift, and the discovery of blockers only when a customer escalates. A new CEM joining the team had no single place to understand what onboarding actually looked like end to end. And there was no way to see — across all active onboardings — which customers were on track and which were quietly slipping.

## What I built

The CEM Onboarding Command Center is an interactive prototype that visualises the full onboarding journey as a task graph. It shows all active onboardings and their health at a glance, surfaces which tasks are blocked and why, and flags customers approaching SLA risk before they breach it.

It's built as a decoupled frontend — no backend, no live data. The purpose wasn't to ship a production system immediately; it was to build something concrete enough that the team could look at it together and have a real conversation about what the process should look like, what data it should pull from, and how it should work. Prototypes like this change the nature of those conversations entirely.

## How I built it

Three hours of focused vibe coding in VS Code with Claude. I described the problem, the data shape (task graph, SLA thresholds, team dependencies), and the interaction model I wanted (interactive, visual, clear status indicators). Claude generated the initial structure; I iterated on the layout and logic until it looked and felt right for the intended audience.

The whole thing runs as a static frontend served via nginx in a Docker container. No API keys, no credentials, no moving parts that can break.

## How long it took

Three hours. That's the honest answer, and it's worth sitting with for a moment: a prototype that would have taken days to spec, design, and build the traditional way was done in a morning. The vibe coding approach isn't magic — you still need to know what you're building and why — but it removes an enormous amount of the friction between idea and artefact.

## Where it stands now

The prototype is live and accessible. It's been used in internal workshops and discussions. It hasn't yet been connected to live data or turned into a production tool, but it's served its core purpose: making something abstract visible enough to be discussed and acted on.

Whether it becomes a full production system depends on resourcing and prioritisation. The prototype makes the case.
