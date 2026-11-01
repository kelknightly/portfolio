# AI Enablement & Training

> Getting people who don't think of themselves as technical to build with AI: a self-serve knowledge base with my own recorded walkthroughs, recorded group workshops, and 1:1 coaching from setup to production at amazee.io; a weekly build-and-showcase session I created and run for DGXC; and active work to make the wider AI community at Mirantis more open to newcomers and to women.

| | |
|---|---|
| **For** | amazee.io workstream leads and teams; DGXC; the Mirantis AI community |
| **Timeline** | April 2026 – ongoing |
| **Status** | Ongoing |
| **Role** | Creator, facilitator and trainer; author of the knowledge base, videos and starter kit |
| **Formats** | Notion knowledge base, recorded video walkthroughs, recorded group workshops, 1:1 sessions, weekly showcases, Slack community, GitHub template repo |

---

## The problem

Most people at a company like amazee.io have heard they should "use AI", and most are stuck on step one. They don't know which approved tool to open, whether their problem needs a script, an automation or an app, how to connect a project to GitHub, or what to do when the AI asks to run a terminal command. Engineers learn this by osmosis. Everyone else needs a path.

The bigger risk is cultural. If the AI conversation is held in rooms that feel like they're only for deep engineers, the people who could get the most out of it (operations, finance, HR, marketing) never walk in.

## amazee.io: the Agentic Engineering programme

I built a complete learning path for workstream leads and their teams, designed so someone with no coding background can go from "I have an idea" to a working tool in production.

**A self-serve knowledge base.** I wrote and organised an *Agentic Engineering* space in Notion:

- **Getting Started**, built around one question: *what are you trying to do?* A decision guide walks through six approaches in order of effort: a native integration, an API connection in n8n, a script, a custom build, an MCP server, or an always-on agent. Each comes with a plain-language explanation, examples from our own tools, and a rule of thumb for when to move up a level.
- **Step-by-step guides** for the parts that stop beginners cold: connecting VS Code to GitHub, creating a project from scratch without the command line, the handful of terminal commands you actually need, and making your first commit and push.
- **My own recorded videos**, showing the whole process end to end: building a frontend dashboard, and writing a script that moves data from one tool to another.
- **Security guidance**, a glossary, a guide to using OpenClaw agents internally, and a page on the role of a supporting engineer when non-developers build with AI.
- **Two trackers**: the agentic projects underway across the company, and how many people actually use the tools that get built (see also the [User Adoption Tracker](./tracking-user-adoption.md)).

**A starter kit repository.** I built a template repo for new projects, so good practice comes built in: AI agent instructions (for Copilot and Claude), a security review template, the OWASP API Top 10, an Okta OAuth implementation guide, a guide to preventing AI hallucinations, Lagoon hosting templates for Node and Python, a monthly maintenance checklist, and automated dependency scanning.

**Recorded group workshops.** I run workshops for workstream leads, and every one is recorded and added to the knowledge base, so people can catch up and new joiners can start from the beginning.

**1:1 sessions, from setup to production.** Group sessions get people started; 1:1s get them finished. I work through each person's specific problem with them, from installing tools and connecting accounts to debugging a script, setting up security and getting it deployed.

You can see what this produces in [Workstream Automations](./workstream-automation-wishlists.md). The Head of HR is learning to run and change his own BambooHR dashboard through weekly sessions with me. A BizOps bookkeeper with no coding background now has a shared script repository to practise in. Marketing works from a shared automation backlog.

## DGXC: Prompt to Prod

Prompt to Prod was my idea, and I've driven it from the start. It's a weekly session where people show what they're building with AI and learn from each other's approaches and mistakes.

As facilitator, I do more than host the call. I encourage people to bring work to show, especially work in progress, so that showcasing becomes a normal habit rather than a performance. Between sessions, the conversation carries on in an *#ai-team* Slack channel where people share tools, wins, questions and things they've read.

As the name says, the aim isn't just to talk about AI but to get ideas into production, and to show everyone in the room what that path looks like.

## Mirantis: opening up the AI Team weekly

amazee.io is part of Mirantis, and Mirantis runs a weekly AI Team meeting. I'm a regular contributor there.

Few amazee.io people attended, and few women. So I set up a 1:1 with the facilitator to talk about opening the meeting up, and wrote up recommendations for them, backed by research:

- Women make up about 30% of AI engineering talent (LinkedIn data in a World Economic Forum paper: 29.4% in 2025).
- Women use generative AI about 20–25% less than men, across 18 studies covering more than 140,000 people (Harvard Business School).
- People often don't put themselves forward because they read the stated requirements literally (Tara Mohr, Harvard Business Review). If a meeting looks like it's for deep engineers only, people believe it, so saying plainly "this is for everyone" changes who comes.

The recommendations were practical:

- add amazee.io people to the invite list, which had been built from the original Mirantis team
- say explicitly that everyone is welcome at any technical level, and back it up with personal invitations ("I'd love you to show what you're working on") rather than open ones
- run an introductory session when a wave of new people joins, and introduce newcomers
- put new voices early in the agenda
- move deep infrastructure questions to a thread afterwards, so that a first-time presenter showing a workflow isn't put on the spot about inference engines and latency under load
- share recordings and transcripts, so cautious newcomers can see what the meeting is like before they join
- post a weekly call for showcases that says all projects and perspectives are welcome

I wrote it from experience. The hardest room to speak up in is the one where you're the only woman, you don't know most of the people, and you're the least technical person there. Small, deliberate changes to how a meeting runs make that room easier for the next person.

## How I approach it

**Start with the person's problem, not the tool.** The Getting Started guide doesn't open with "here is Copilot". It opens with "what are you trying to do?", and often the answer is a native integration that needs no code at all.

**Show the whole path.** Most AI tutorials stop at a demo. Mine go from a blank folder to a committed, deployed and secured project, because the gap between "it works on my laptop" and "the team uses it" is where beginners give up.

**Make it safe to be new.** That means recordings to catch up from, 1:1s where no question is too basic, showcases where unfinished work is welcome, and meetings designed so that newcomers aren't put on the spot.

**Hand over ownership.** The measure of success is people building without me. The Head of HR running his own dashboard, and BizOps keeping its scripts in its own repository, matter more than anything I build myself.
