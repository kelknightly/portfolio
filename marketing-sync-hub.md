# Marketing / BizDev / Sales Weekly Sync Hub

> A live meeting dashboard for the amazee.io Marketing, Business Development, and Sales weekly sync — replacing static slide decks with real-time data panels and a persistent history anyone can review.

| | |
|---|---|
| **Built for** | amazee.io |
| **Timeline** | ~1 week initial build, plus a significant refactor |
| **Status** | Live |
| **Team** | Solo |
| **Stack** | Next.js, TypeScript, Prisma, PostgreSQL, Docker, TailwindCSS |

---

## The problem

Weekly sync meetings at amazee.io between Marketing, BizDev, and Sales were running on a mix of prepared slides and live HubSpot screenshots. This created a few recurring frustrations: slides went stale before the meeting started, whoever prepared them had to spend time gathering and formatting data, and anyone who missed a meeting had no reliable way to catch up on what was discussed or decided.

There was also no consistent structure. Different weeks had different formats depending on who prepared the deck. Important metrics appeared in different places. Comparisons to previous weeks were done from memory.

## What I built

The Marketing / BizDev / Sales Sync Hub is a live dashboard designed to run during the weekly sync meeting itself. Each workstream — Marketing, BizDev, Sales — has its own data panel, populated with live metrics rather than screenshots. The panels are modular and can be reordered via drag-and-drop, which turned out to matter more than I expected: different meeting leads have different priorities and wanted to control the flow.

At the end of each meeting, the dashboard takes a snapshot of the data and pushes it to Notion — including any tasks or action items captured during the session. That snapshot becomes the permanent record for that week, accessible to anyone who missed the meeting or wants to compare over time.

## How I built it

Built solo with Claude and VS Code, in my by-now familiar pattern of describing the problem and the desired outcome and iterating toward it. The initial build took about a week. Then the team used it for a few weeks and came back with real feedback — specifically that the layout needed to be transposable and the modular block system needed to be more flexible.

That feedback resulted in a significant refactor. I rebuilt the layout system to support drag-and-drop section ordering and redesigned the block components to be genuinely modular — each one can be shown, hidden, or repositioned without affecting the others. The Notion integration for snapshots and task pushing was added at this stage too.

Refactoring in response to real user feedback is different from speculative refactoring. This was worth doing because the team had actually used the thing and knew what they needed.

## How long it took

About a week to the first live version, then another period of work for the refactor. Total time invested is probably two to three weeks across both phases.

## Where it stands now

Live and running. It's become the default format for the weekly sync — nobody's gone back to slides. The snapshot history in Notion means there's now a consistent, searchable record of what the team has tracked week over week, which is useful in its own right for planning and retrospectives.

This project is a good example of what it means to build for actual users rather than imagined ones. The first version was correct. The second version was better because real people used it and said what they actually needed.
