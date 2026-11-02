# Context, Checks, Judgment: exploring Epoch AI's public data

> A day spent seeing what [Epoch AI](https://epoch.ai/) makes public, and what I could build on top of it. I took apart their Automation Reports grading notes, wrapped their Data Hub in a small MCP server, and rebuilt a changelog of their datasets from Internet Archive copies. Breaking data down like this is how I get to know it.

| | |
|---|---|
| **Built for** | Personal exploration |
| **Timeline** | November 2026 (one day) |
| **Status** | Live: [the interactive page](https://claude.ai/artifact/WWXPvHL1NDg1xStXs9So78) |
| **Team** | Solo. I direct and decide; Claude Code writes the code. |
| **Stack** | Python, DuckDB, MCP Python SDK, Claude Code, Claude Artifacts, Internet Archive CDX API, inline SVG |

---

## Why I did this

Epoch AI publishes a lot openly: datasets under CC BY 4.0, a benchmark where human graders explain every point they take off, and the code behind some of their estimates. I wanted to see what was accessible and what I could do with it.

When I meet a new dataset, I sort it, diff it, build a small tool around it and see what falls out. This page is what fell out.

## What I looked at

- **Epoch Automation Reports.** Six AI agents attempted eleven real Epoch tasks, and human graders marked each output against a rubric. All 381 failed criteria come with a grader's note.
- **The Data Hub.** The CSV and ZIP files behind the Download buttons on [epoch.ai/data](https://epoch.ai/data).
- **The Internet Archive.** Archived copies of two of those CSVs going back to 1 September 2026, which let me see how the data changes over time.

## What I made

**The misses, sorted by cause.** Two independent AI coders labelled every failed criterion as *context* (knowledge Epoch staff carry), *check* (a rule a script would catch) or *judgment* (needs a better model or a person). They agreed on 90.6% of them (Cohen's κ 0.84), and I settled the rest by reading each note. About 40% of the points lost were context or check misses. A chart with switches shows each system's score with those misses added back, as an upper bound rather than a forecast. My scoring reproduces Epoch's published numbers.

**Grader notes as rules.** Notes like *"Should use pink instead of orange"* and *"Same tokens vs. requests claim mentioned 3 times"* read as instructions once you turn them round. I wrote them into a small skill for drafting Epoch-style Data Insights.

**A read-only MCP server.** It downloads the public Data Hub files, loads them into DuckDB as 23 tables, and gives an AI agent five tools: `list_datasets`, `describe_table`, `query`, `search_publications` and `whats_new`. Every answer carries its source and licence, and only a single `SELECT` is allowed. I checked it through a real MCP client session, and a test `DROP TABLE` was refused.

**A checker, tried on a real draft.** A 17-rule script for Data Insights: word limits, numbers that repeat across sections, too many numbers in one paragraph, palette order, linked sources, and a search for similar published pieces. Run on Claude Fable 5.1's actual Polling submission from the benchmark, it flags three of the five things the grader marked down, partly catches a fourth, and can't help with the last, which was whether the topic matters to the audience. That one needs a person.

**A changelog, rebuilt as a diff.** The Data Hub doesn't publish a changelog, so I compared 19 archived copies of the notable models file and 4 of chip sales, field by field. The page replays nine weeks of changes as a side-by-side diff of the real rows. A few highlights:

- Inkling's compute estimate rose about six-fold when its 6ND calculation gained the factor of 6, which moved it across the 1e25 FLOP line.
- Country and company names were standardised ("NVIDIA" became "Nvidia" on 32 rows).
- A chip sales quarter flagged as incomplete was filled in.

Where a chip sales change lines up with a commit in Epoch's public [ai-chip-counts](https://github.com/epoch-research/ai-chip-counts) repo, the page links it.

## What I learned

- **Epoch already works this way in places.** The ai-chip-counts repo has a validation script and an `AGENTS.md` that tells coding agents how to model and write. Seeing that made the "context and checks" idea feel less like a theory and more like a pattern worth extending.
- **The page got simpler as I went.** The first version was a wall of text. The useful version shows one thing per section, such as numbers bolded in a faded paragraph or a diff with only the changed words lit up.
- **Getting the details right took most of the time.** Along the way I caught myself:
  - counting live files as archived copies
  - describing a quarter being completed as a ×5 revision
  - recording the demo calls without going through the MCP protocol

  Each one was fixed before the page went public.

## Limits

- Everything here comes from public material. Epoch's internal guides and templates would sharpen every rule.
- The context, check and judgment labels are my codebook's judgment calls. Epoch's graders might sort some misses differently.
- The ceiling is an upper bound: real fixes won't catch everything, and they could introduce new problems.
- Dates in the changelog are when the Internet Archive saved a copy, so each change happened on or before that date.

## Where it stands

The [page](https://claude.ai/artifact/WWXPvHL1NDg1xStXs9So78) is public. The code (MCP server, skill, checker and diff) isn't published yet.

This is independent work. It isn't affiliated with or endorsed by Epoch AI, and nothing in it is Epoch's grading. The data, rubrics and grader notes are Epoch AI's, under CC BY 4.0.
