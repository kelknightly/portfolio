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

Three hours of focused vibe coding in VS Code with Claude. I described the problem, the data shape, and the interaction model I wanted (interactive, visual, clear status indicators). Claude generated the initial structure; I iterated on the layout and logic until it looked and felt right for the intended audience.

The onboarding process is modelled as a **Directed Acyclic Graph (DAG)** — which is the right data structure for this kind of problem. Each task is a node. Each dependency between tasks is a directed edge: Task B cannot start until Task A is complete, and the direction is meaningful. "Acyclic" matters here because onboarding steps don't loop back on themselves — the graph has a clear start and a clear end, and every path through it moves forward. This structure makes it possible to reason about things that a flat checklist can't express: which tasks are currently unblocked, which ones are blocking others, and what the critical path to completion looks like.

That last point is what makes SLA risk surfacing possible. By traversing the DAG from the current state forward, you can calculate whether the remaining path — given current blockers — can still complete within the SLA window. A task slipping on day three might not matter if it's not on the critical path. A task slipping on day three that has four downstream dependencies almost certainly does. The graph makes that visible in a way that a status field on a Jira ticket never could.

The whole thing runs as a static frontend served via nginx in a Docker container. No API keys, no credentials, no moving parts that can break. The DAG is defined in the frontend data layer — which is intentional for a prototype: it makes the structure easy to inspect, modify, and discuss without needing a database or an API to change a dependency.

## How long it took

Three hours. That's the honest answer, and it's worth sitting with for a moment: a prototype that would have taken days to spec, design, and build the traditional way was done in a morning. The vibe coding approach isn't magic — you still need to know what you're building and why — but it removes an enormous amount of the friction between idea and artefact.

## Where it stands now

The prototype is live and accessible. It's been used in internal workshops and discussions. It hasn't yet been connected to live data or turned into a production tool, but it's served its core purpose: making something abstract visible enough to be discussed and acted on.

Whether it becomes a full production system depends on resourcing and prioritisation. The prototype makes the case.
