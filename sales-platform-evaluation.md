# Sales Engagement Platform Evaluation

> A structured evaluation of 20 prospecting and sales engagement platforms for amazee.io — from desk research and a scored comparison matrix to vendor calls, live demos and a hands-on trial — ending in the selection of Amplemarket as the single tool for prospecting, LinkedIn and email outreach.

| | |
|---|---|
| **Built for** | amazee.io |
| **Timeline** | Early 2026 (scored comparison: February 2026) |
| **Status** | Delivered; Amplemarket selected and rolled out |
| **Type** | Tool evaluation, vendor due diligence & selection |
| **Platforms evaluated** | 20, including Apollo.io, Amplemarket, ZoomInfo, Clay, Salesloft, Outreach, Cognism, 6sense, 11x and LeadLoft |

---

## The problem

amazee.io's outbound work was split across separate tools: LinkedIn Sales Navigator for finding people, and Dripify for automating LinkedIn outreach. Each tool held a slice of the picture, and every extra tool is another login, another bill and another place for prospect history to get lost.

The goal was to find one platform that could replace both Sales Navigator and Dripify, catch buying signals at the right moment, write good first drafts with AI, and sync cleanly with HubSpot. The sales engagement market is crowded and every vendor claims to do all of this, so the real work was finding out which claims held up.

## What I did

The work ran in the same three phases I used for the [Documentation Transformation](./amazeeio-documentation.md): research, hands-on evaluation, then a decision the team could stand behind.

**Desk research and a scored matrix.** I built a comparison of 20 platforms, using Apollo.io as the benchmark because it's the best-known all-in-one tool. I scored each one on the same criteria:

- Ease of use
- Could it replace Sales Navigator? (full, partial or no)
- Could it replace Dripify? (full, partial or no)
- "Right time" tracking: which buying signals and alerts it offers (job changes, funding news, intent data, website visits)
- Could it be our one-place solution, or would it be another tool in the stack?
- How far its AI features reach, and what they actually do
- Where it sits competitively, and who it's really built for
- My own rating, with written reasons

The matrix narrowed the field quickly. Some tools were strong but solved only half the problem: Salesloft, Outreach and Instantly handle engagement but have no prospecting database; Lusha, UpLead, RocketReach and Seamless.ai are mostly databases. Some were built for a different buyer: 6sense for account-based marketing at enterprise scale, Cognism for EMEA phone data, Clay for custom data work that needs an operator. And 11x, which replaces SDRs with fully autonomous agents, went the other way from what we wanted: more automation, but less of a human voice.

**Calls, demos and trials.** Five platforms scored moderate or higher: Apollo.io, Amplemarket, ZoomInfo, FlashIntel and LeadLoft. With the shortlist I went under the hood: calls with their sales teams, live demos, knowledge bases and roadmaps, and trial accounts where they were offered. I took a written list of questions into each conversation, so every vendor answered the same things: could we manage different ICPs as separate segments, could we cap how many leads a sequence contacts each day, how long was the trial, and how good was the AI on LinkedIn messages, not just email.

Those questions did the sorting. LeadLoft could cap daily contacts and segment ICPs, but it expected us to keep paying for Sales Navigator, its AI personalisation on LinkedIn was limited, and its prospect filters could only be changed by rewriting AI prompts. ZoomInfo had every feature but felt heavy for a team our size. FlashIntel was promising but newer, with a steeper learning curve.

**The decision.** Amplemarket came out on top, edging Apollo.io. It had the same breadth of features with a cleaner interface, many kinds of change signals, good data with transparent sources, full LinkedIn coverage (including a browser extension for event attendee lists), complete message automation across email and LinkedIn, a native two-way HubSpot integration, and a way to reactivate closed-lost deals. Before committing, I booked a second demo to close out the open questions and checked the terms of the 14-day trial.

## How I approached it

The hardest part was resisting the demos. Every sales engagement vendor gives a great demo. A scored matrix and a fixed set of questions made the vendors comparable, and kept the decision about our needs rather than the slickest pitch.

I also wrote my opinion down, separately from the feature scores. A tool can tick every box and still be exhausting to use every day, and a team won't adopt a tool it dislikes. Writing down why I rated each one the way I did ("too email-focused", "no prospecting database", "would just be another tool in the stack") made the gut feel open to discussion instead of hiding it.

The bar was "replace two tools and connect to HubSpot", not "has the most features". That ruled out a lot of strong products quickly, and it's why the final choice was easy to explain.

## Key deliverables

- A scored comparison matrix of 20 platforms across 8 criteria, with written reasoning for each rating
- A standard question list used in every vendor call and demo
- Calls, demos and trials with the shortlisted vendors, including a second Amplemarket demo to close out open questions
- A recommendation to consolidate Sales Navigator and Dripify into Amplemarket
- Rollout and team enablement, continued in the [AmpleMarket Team Hub](./amplemarket-team.md)

## Where it stands now

Amplemarket is amazee.io's outreach platform, and I own it. Since the selection I've connected its replies to HubSpot through n8n ([RevOps Automation](./amazeeio-revops-automation.md)), had Claude work lead lists through Amplemarket's MCP server, and started building the team hub that will let others use it on their own.

Like the documentation project, this one has no codebase. The output is a decision and the evidence behind it, which means the team doesn't have to re-argue the choice six months later.
