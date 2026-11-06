# Flight School: Learning an AI Platform from Zero

> A custom course I built for myself in two days, so that a non-engineer could follow a daily engineering standup about an AI inference platform. Fifteen modules with custom diagrams, an animation and real photos, a quiz on every module and a final mixed quiz, Claude marking my explanations in my own words, and a live page of my own tickets and pull requests that keeps itself current.

| | |
|---|---|
| **Built for** | Myself, then my colleagues at Mirantis |
| **Timeline** | November 2026 |
| **Status** | The course and the tickets page are live. A version for colleagues (sign-in, private progress, tracks by role) is built and waiting to be installed on the company's servers. |
| **Team** | Solo. I decided what the course needed to teach and checked every page; Claude agents researched the sources and wrote the pages under my direction. |
| **Stack** | HTML/CSS/JavaScript in a single Claude artifact, hand-drawn SVG, Claude (for marking and questions), Jira through the Atlassian connector, the artifact's own database, a scheduled Claude Code job. The colleague version: Node, Hono, PostgreSQL, single sign-on, the Claude API. |

> **A note on names.** The platform this course teaches hasn't launched yet, so I can't name it or its parts here. I describe them in general terms instead: "the platform", "the enterprise AI gateway", "the operations console". The console has its own case study: [AI Platform Operations Console](./flightdeck.md).

---

## The problem

In November 2026 I started attending the daily standup of the AI engineering team behind the platform. The team lead puts the Jira board on screen and walks through it, one person at a time. The tickets are full of words like prefill, KV cache, rack gateway and disaggregated serving, and the team changes direction most weeks.

I run one of the projects on that board, so I need to follow all of it, not just my own tickets. There was no course for this. The knowledge lived in the engineers' heads, a fast-moving internal wiki and the code. Reading those cold doesn't teach you much, because nothing tells you which parts matter or what the words mean.

## What I built

**A course, fifteen modules in four parts.** Part 1 covers the basics: what a token is, what a GPU and a rack are, and how one request travels through the system and back. Part 2 covers the company: who buys the platform and why, the enterprise AI gateway sold alongside it, the machines, the team and where its work is written down, and the AI agents that respond to alerts. Part 3 covers the operations console I run: what it's for, every tab, and what it's built on. Part 4 is standup itself: how to give an update, and a phrasebook of what people say and what they mean.

Every module follows the same shape. It opens with one picture that carries the idea, then short sections with a bold lead, then "words you'll hear" with plain meanings, then "what people say in standup" translated into plain English. Each module is dated and says what it was checked against, because a course about a team that moves weekly goes stale fast.

**Custom diagrams and an animation.** The pictures were drawn for this course, not taken from a library:

- **Who sells what to whom:** two sales, two buyers. The kit goes one way and the tokens go the other, with bars drawn to scale showing what one GPU earns rented out versus selling tokens.
- **A request's round trip,** animated. A dot travels a loop through the four stops a request passes, pausing at each while the caption explains that stop, then the answer streams back as a train of tokens along a green arc. It has a pause button, and it stays still for anyone whose device asks for reduced motion.
- **Rack, tray, GPU:** a zoom from a whole rack of 18 trays, to one tray of four GPUs, to one chip and its memory. The rack is drawn by code, because drawing 18 rows by hand is just noise.
- **The gateway as a switchboard:** staff on one side, suppliers on the other, and one desk in the middle doing four checks.
- **Who chooses what:** a fan that narrows from the gateway to one rack, one model and one copy of that model, with handwritten-style notes in the margin answering the questions I'd asked.
- **Three photographs for scale:** a real rack, an open tray, and a consumer graphics card to compare it against.

**A quiz on every module, and a final mixed quiz at the end.** 77 multiple-choice questions in total. Every answer explains why it's right or wrong, the answer options are shuffled so the right one isn't always in the same place, and a module only gets its tick in the course map at four out of five. The last module is a nine-question mixed quiz across the whole course, with a pass mark of eight.

**"Explain it back."** Under every quiz is a box where I explain the idea in my own words. Claude reads it against that module's lesson and tells me what I got right, the single most important thing I missed, and how to fix it. The same box takes questions. This is where I actually find out what I understand, and it's the part I use most.

**A glossary and a links page,** so a word heard in standup can be looked up in seconds.

**My tickets and pull requests, live.** The last page lists every ticket I've created or been assigned and every pull request I've opened for this work, one card each, with one plain-English line saying what it is and who it's waiting on. That's the line I read out at standup. Ticket status, title and assignee are read straight from Jira every couple of minutes. Pull requests are refreshed by a scheduled Claude job on my machine each morning, and the page shows when it last ran, with a red cross if it failed or is more than a day old. Cards that changed since I last looked get an orange dot, and hovering over the dot says what changed. The project's agent instructions say that any agent that creates or changes one of my tickets or pull requests must update this list in the same session, so it never drifts from reality.

## How I built it

I wrote the brief the way I want things explained to me: plain words, no jargon, one picture per idea, short sections, only the numbers that change a decision. Then I read every module as a learner, and every place I got confused became a fix. Some of the pictures exist because of a specific misunderstanding. The "who chooses what" fan came from mixing up the two different gateways in the system, which an earlier page had blurred together.

Every fact was checked against three sources: my own project notes, the code, and the engineering team's wiki. Where they disagreed, the wiki won and the course says so.

When colleagues asked for access, I didn't want them seeing my quiz answers, and a colleague pointed out that the sales team would benefit too. So I had a separate version built as a small internal website: company single sign-on, progress kept private to each person, questions shared without names, and five tracks (sales, gateway, engineering, operations, other) that mark some modules as core and rewrite pages for each audience. Claude marks explanations against the two or three core ideas of each module rather than a model answer, and a pass is a pass with no "but".

## Where it stands now

The course and the tickets page are live.

The colleague version is finished and waiting on the platform engineer to install it on the company's servers and approve its Claude budget. Once it's live, the next step is seeing whether sales gets as much out of it as I did.
