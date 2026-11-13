# Atlas

> A VS Code extension that shows what my AI agents are doing as they do it, and holds them to rules. Every model call and tool call from Claude Code, my gateway scripts and a self-healing agent lands in ClickHouse within about a second. Atlas draws it as a live map, scores it against policies, refuses gateway calls that break them, watches for leaked secrets, and answers questions about the data in plain words.

| | |
|---|---|
| **Built for** | Personal (my own machine, across one personal and two work logins) |
| **Timeline** | November 2026 |
| **Status** | Live on my machine. The collectors run as background services; the extension is a first version |
| **Team** | Solo, built with Claude Code |
| **Stack** | ClickHouse, Node.js (no dependencies), a VS Code webview extension with no build step, systemd user services, OpenAI-compatible model gateways, Claude Sonnet 5.5 for the chat |

![The Atlas map: the agent in the centre, with lines to the model, files, shell, web and subagents](./screenshots/Atlas%20-%20Map.png)

*Every screenshot here is the extension in its Demo view, which reads a masked copy of the data: work identities, projects, hosts and keys are replaced with stable placeholders.*

---

## The problem

I run AI agents all day: Claude Code under three different logins, evaluation scripts that call open-weight models through a gateway, and [Canary](../canary/canary.md), an agent that repairs failing workflows without me. I could not answer basic questions about any of it. What did that agent touch? Who approved it? What left my machine, and to where? What did a pull request cost?

The data existed. Claude Code writes a detailed transcript of every session to disk. But it sat in 277 JSON files, 1.7 GB of them, and the other agents logged elsewhere or not at all.

## Getting it into one place

Three collectors write to one ClickHouse table, one row per model call, tool call or prompt:

- **Claude Code transcripts.** A service tails the transcript folder and is about a second behind. Each row is labelled with the login that was active for that request, so a session that switches account part-way is split correctly.
- **A local gateway proxy.** Scripts that call an OpenAI-compatible gateway change only their base URL. The proxy passes the call through, records tokens, cost and a fingerprint of the key, and never stores the key.
- **Canary's healer traces**, pulled from its GitHub Actions runs.

The store is metadata only. It keeps which programs a shell command ran and which folder it worked in, not the command. It keeps a file's path, not its contents. No prompt or response text is stored. That decision came first, because a telemetry store that copies everything becomes the most sensitive thing on the machine.

Each row also carries what a security reviewer would ask for: the identity, the credential, the kind of power used (read, write, shell, web, a connector), which trust boundary the call crossed, where it went, and who decided it could run: a person, a fixed rule, the permission mode or an AI classifier.

As of this write-up: 35,505 events from 284 sessions, across 10 models.

## The map

The centre is the agent: Claude Code on my machine. Around it are the things it can reach. Files, shell, skills and subagents sit inside a dashed ring marked "this machine"; the model provider, the web, connected services and publishing sit outside it, so anything crossing the ring is data leaving.

A request lights up the line it travels on. Subagents get their own colour and their own tab, and selecting one scopes the totals and the feed to it. Selecting a node filters the feed to that kind of activity.

Building it corrected something I had half wrong. I thought of my prompts as going "to the model". They go to the agent, which makes many trips to the model for one prompt, each carrying the whole conversation.

## Controls

### Policies, and one place that can say no

![Seven policies, three enforced at the gateway proxy and four scored against recorded activity](./screenshots/Atlas%20-%20Policies.png)

Policies are rows in a table, each one a condition over recorded events. Three act at the gateway proxy, which checks them before a call leaves the machine and can refuse it: a model allowlist, a daily budget and a list of blocked keys. The other four have nothing in the path, so they can only be scored afterwards. The card says which is which, because a rule that can only be reported on is not a control.

Every rule has an Enforce, Monitor or Off switch, and every change to a rule is recorded.

### What agents actually did

![Observed access: projects down the side, kinds of power across the top, with counts and blocked calls](./screenshots/Atlas%20-%20Observed%20Access.png)

This grid is built from behaviour, not from configuration. Each cell is how often agents used one kind of power in one project. A badge marks calls that were stopped, with the worst kind first: a red badge means a classifier flagged a security risk, and selecting it opens those events.

### Who can read the telemetry

![Logins, quotas, the row policies ClickHouse applied, and the queries it refused](./screenshots/Atlas%20-%20Access%20to%20the%20Telemetry.png)

The store has several ClickHouse logins: an owner, a read-only demo login that sees only a masked copy, and one read-only auditor per identity, each with a row policy so it sees its own rows and nothing else. Each has a quota.

The extension does not enforce any of this. ClickHouse does, and the card shows its evidence: which row policy was applied to each login's reads, taken from the query log, and every query it refused. A "View as" control in the top bar switches login, so the same page can be read as each one.

### An audit log that cannot be quietly edited

![Audit log integrity: four checks passed, and the chain of sealed blocks](./screenshots/Atlas%20-%20Audit%20Log%20Integrity.png)

Every row written leaves a SHA-256 fingerprint in an append-only log. Every five minutes the new fingerprints are sealed into a block whose hash also covers the block before it, and that hash is copied to a file outside the database. A button changes one sealed row directly in the table, shows the check failing, and puts the row back.

It proves that nothing sealed was altered or removed. It cannot prove the collector recorded everything, and the card says so.

### Leak watch

![Leak watch with a simulated leak: secrets in commit history, with what to do about them](./screenshots/Atlas%20-%20Leak%20Watch.png)

A scanner runs every ten minutes over the transcripts and the git repositories agents worked in. It looks for secrets in chats and in commit history, watched names and money figures in repositories, agents reading secret files, and a repository worked on under the wrong login. Findings of one kind show as one alert, with why it matters and what to do in order.

It never stores what it finds: only a secret's first four characters, its length and a hash. A command plants a made-up leak to show an alert arriving; the screenshot above is that simulation.

## Cost

![The Cost tab: list-price totals, spend by day and model, and budgets per identity](./screenshots/Atlas%20-%20Cost.png)

Spend by day, model, identity, project, session, commit and pull request, with a budget and a month-end forecast per identity.

The first version of this tab alarmed me: it said I had spent several hundred dollars in a month on a 100-dollar subscription. The numbers were right and the label was wrong. They are tokens times the public API price, not a bill. The tab now says so, and shows the flat fee actually paid beside the list-price figure. Nearly all of that figure is cache reads: long conversations re-reading their own context on every call.

Pull request cost had a second problem. A call counted toward the next pull request its session opened, so one small pull request opened from a long session was charged for hours of unrelated work. It now counts only calls made while the session was working in that pull request's repository. That took one figure from $68.80 to $2.21.

## Asking it questions

![The Chat tab: a question, the table that answers it, and a follow-up](./screenshots/Atlas%20-%20Chat.png)

The Chat tab takes a question in plain words. Claude Sonnet 5.5 writes one SQL query, ClickHouse runs it, and the result is shown as a table. An answer takes two to four seconds.

I picked the model with a small eval: 30 questions with known answers, asked twice of each model against the masked data. Sonnet 5.5 got all 30 right. Haiku 5.5 missed one with thinking on or off, and without thinking two of its queries failed in ClickHouse and had to be retried. Sonnet also wrote about half as many tokens per answer as Haiku with thinking on, so the bigger model was not the more expensive one here.

The model only writes the query. It never sees the results, and the query runs as a read-only login that follows "View as", so an auditor asking the same question gets only its own rows. Safety does not depend on the model behaving: asked to drop a table, ClickHouse refuses, and the refusal shows up on the Controls page.

## What it found

Pointing this at my own machine was more useful than I expected, and less flattering.

- **Almost nothing was decided by me.** Of 17,879 tool calls, an AI classifier approved 74% and I approved 0.2%. The rest ran on defaults, the permission mode or a rule.
- **No shell command ran in a sandbox.** That policy scores zero.
- **One secret was in a chat transcript.** The agent's redaction had missed a password inside a connection URL.
- **An agent had read a `.env` file in one project.**
- **A work repository had been opened under the wrong login.**
- **A public repository had names in its history that should not be public.**

None of this was visible before. Each finding is now an alert or a scored policy in Atlas with the fix listed beside it, and I am working through them.

## Limits

- Enforcement covers only calls that go through the proxy. Claude Code's own calls go straight to the provider, so for those Atlas can report but not refuse.
- The permission details come from undocumented fields in the transcripts and could change without notice.
- Leak watch matches known shapes. A password typed in a sentence is missed, and customer names are found only if they are on a watch list.
- Read or write for a connector tool is judged from the tool's name.
- Encryption at rest is not set up on the database.
- It is one day's build on one machine, checked mostly in a browser preview of the extension's panel.

## What I'd take from it

The useful controls were the ones enforced below the application. The extension asks; ClickHouse decides who reads what, and the proxy decides which calls leave. Everything the page shows about access is evidence from those two, not a claim by the page.

The other lesson is that observing agents turned up problems that no policy document would have. I would not have guessed that I personally approve one tool call in five hundred.
