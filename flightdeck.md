# AI Platform Operations Console

> A real-time operations console for an AI inference platform: callers, gateway, models and GPUs on one screen, drawn like air traffic control. AI agents work the incidents and people supervise them. It runs on live GPU racks, never invents a number, and says plainly what the data can't tell it.

| | |
|---|---|
| **Built for** | The AI platform team at amazee.io (a Mirantis company) |
| **Timeline** | September 2026 – ongoing |
| **Status** | Live on a lab rack and a production GPU rack; in active development |
| **Team** | I lead the project and decide what gets built; Claude agents write the code under my direction. The platform's engineer built v1 and reviews each step. |
| **Stack** | HTML/CSS/ES modules (no framework), Python aggregator, ClickHouse, WebSockets, Kubernetes, GitHub Actions, LiteLLM, Jev (TypeSafe) for alert triage, Claude |

---

## The problem

The platform serves open models from racks of NVIDIA GPUs to real users through an API gateway. When it slows down, the cause could be any of several things: a caller sending a burst, a model hitting the point where adding load stops adding throughput, a GPU running hot and throttling, or an engine restarting. The existing dashboard was a live diagram of the system. It couldn't tell congestion from a fault, couldn't rewind, and gave no picture of what the platform's AI agents had been doing about it over time.

There was a second problem underneath. When something breaks on a rack, it doesn't raise one alert, it raises a swarm. One machine leaving makes every pod on it shout. Each alert started its own investigating agent, so in one 44-alert storm there were 44 agents working the same fault.

## What I built

**The console.** Version 2 borrows its whole visual language from air traffic control, because that's a profession that has already solved "many things moving at once, a few of them in trouble":

- **Scope:** every model is a target on a radar-style plot, positioned by how close it is to its own breaking point, so a model about to saturate is visible before it does.
- **Recorder:** one model's full record, with every measurement on one clock and ordered from cause to effect: machine, incoming traffic, cache, pressure, service, outcome. Red marks only the moments a line was past its limit, so you can see which link failed first.
- **The rack:** the machine itself, one row per GPU tray and one cell per card, with each model as a coloured region. It's always one screen, whatever the number of models.
- **Desk and Strings:** the agents' work. Open incidents are progress strips in bays, the way controllers track aircraft. A thread chart shows which agent touched which model over the hour, and each red tick marks the moment an agent asked a person for help.
- **Monitor:** forecast demand against capacity, with a line marking the last moment you can still scale in time.

Every board can pause and rewind together, because the past is read from the store, not reconstructed. A **Simulate** button lays five minutes of invented traffic and agent work over the real snapshot for demos. That simulation runs in one browser tab only, is stamped SIMULATED everywhere it shows, and removes itself when it ends. The board's standing rule is that it never draws invented data as if it were measured.

**The doorman for alerts.** To stop one fault spawning a dozen agents, alerts now pass a doorman before any agent starts. Free, exact rules go first: the same alert again, the same engine, the same machine, a flood. Jev, a small and very cheap decision model from TypeSafe, is asked only about the alerts the rules can't place, with one typed question: "is this the same fault as an open problem, or a new one?" It answers with a confidence, and we only join an alert to a problem at 0.6 or above, because burying an alert in the wrong problem is worse than one agent too many. Every decision is written down with who made it and how sure it was.

## How I built it

I ran this the way I run every build: sit with the person who owns the problem, understand what they need to see, then build the version I believe in and put the working thing in front of them. The engineer who owns the platform wanted decisions made and shown running, not questions asked in the abstract, so every design call arrived as a board on real data with the reasoning written beside it.

The code is written by Claude agents working under my direction, across a lab rack and a production NVIDIA GPU rack that also serves models to real users. That second machine meant strict operating rules, which I wrote into the project for every agent to follow: change things only in our own namespace, no load tests without an explicit yes for that run, never touch real incidents, and keep secrets out of chat, logs and git.

The evaluation work is the part I'm proudest of. When Jev first ran over the production rack's real alerts, it made 32 joins and 9 were wrong. I read every one by hand and traced all nine to one cause: a problem that had been quiet for two days looked recent because its original alert happened to close minutes before an engine swap. We fixed what the model was shown that same day. After the fix, 221 alerts became 67 problems instead of 97, and every join checked out.

Then I tested whether a bigger model would do the job better. I built a harness that asked Claude Haiku and Claude Sonnet the identical question at every point Jev was asked: 350 questions across both racks, with every disagreement read against the real timeline. Sonnet's judgement was better: no wrong joins, and about fifty correct joins Jev missed. But its confidence was a round number it typed, almost always 0.6 or 0.8, while Jev's was a measured probability. Our safety bar only means something with a real probability. Haiku answered in the requested format only half the time. So Jev stays at the door. Most of what Sonnet caught became a free rule ("different alerts in the same second belong together"), and Sonnet is one switch away if the team ever prefers its judgement. The whole test cost about two and a half dollars.

## Where it stands now

Live. The console reads real data on both racks, and Jev triages the production rack's live alerts. Its latest additions show answers travelling back to the caller who asked, and read each model's limits from where they originate instead of from a list we keep. Next, it moves to the platform's model registry once a read-only key is available.

This project brings together most of what I care about: an interface that makes a complex system legible to the people running it, AI used only where rules can't do the job, and an evaluation honest enough to choose the cheaper model for a good reason.
