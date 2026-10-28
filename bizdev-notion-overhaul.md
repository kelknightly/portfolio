# BizDev Notion Overhaul

> A zero-loss restructure of amazee.io's Business Development workspace in Notion: loose pages turned into seven linked databases, every original page preserved, every change recorded and reversible, and a verifier that proves nothing was lost.

| | |
|---|---|
| **Built for** | amazee.io (Business Development team) |
| **Timeline** | July 2026; the migration itself ran in a single day |
| **Status** | Delivered |
| **Team** | Solo, built with Claude |
| **Stack** | Python, Notion API, an append-only audit ledger, JSON backups |

---

## The problem

The Business Development teamspace had grown the way most Notion spaces do: pages for each partnership, agency programme and funding scheme, meeting notes scattered between them, and no consistent way to answer simple questions. Which partnerships are active? Who did we last speak to, and when? Which targets are we tracking, and against what?

The team wanted databases they could filter, sort and link: partnerships connected to contacts, contacts connected to touchpoints, targets connected to the initiatives behind them. But the existing pages held real history, including meeting transcripts and uploaded files, and none of it could be lost in the move.

## What I built

A seven-database structure under one BizDev home page: Partnerships, Contacts, Touchpoints, Targets & KPIs, Initiatives, Programs & Funding, and a Knowledgebase. Between them they carry 210 properties and 26 saved views. Around them are hub pages for the weekly sync, vendor partnerships and the agency programmes. There's also a "Data Gaps & Decisions" page listing what only a person can resolve.

The migration seeded 88 rows and moved 21 original pages into their new database rows with their content preserved byte for byte. Eight old container pages went to an archive. **Nothing was deleted.** The verifier checked that 478 original text units were still present, three files were byte-identical, and all 39 pages were reachable.

## How I built it

Safety came first, because a migration like this is easy to get mostly right and impossible to undo by hand.

- **A full backup before anything else:** raw JSON for every page, block, database and row, plus the uploaded files and meeting transcripts.
- **Dry run by default.** Every script prints exactly what it would do and only writes with an explicit flag, and the dry run and real run print the same summary, so any difference is itself a warning.
- **One guarded write path.** Every change goes through a guard that checks the target is inside the BizDev space twice, once against a list and once by walking up the page tree. It refuses deletes outright and won't touch a list of pages that must stay as they are.
- **An append-only audit ledger** that records the intent before each change and the outcome after, so a run that died mid-write is obvious.
- **A rollback runbook,** with a one-command undo for every phase.

The other half of the work was finding out what Notion's API actually allows, rather than what the documentation says. I probed it first in a sandbox: 23 capabilities, all checked before touching real content. Several findings changed the design. The API can move pages between parents, which is what makes a zero-loss migration possible at all. But a formula can't reference another formula through the API, even though the Notion app allows it. 29 of 36 formulas failed until I built a small macro system that inlines them, and schema creation had to run in five ordered passes.

## Where it stands now

Delivered. The team works from the new databases, the originals sit in the archive, and every phase can still be undone. Each partnership and contact already carries the HubSpot fields that will let an n8n workflow keep them in sync with the CRM, which is the next piece of work.

This is the same instinct as my other data work, applied to a team's knowledge rather than a pipeline: prove what you have before you change it, make every change reversible, and verify afterwards that nothing was lost.
