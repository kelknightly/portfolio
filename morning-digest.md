# The Morning Digest

> A one-page newspaper about our household's money and the day's AI news, made each morning before we wake up. An open-weight model writes the headlines, plain code produces every number, and the plan is for the printer to have it ready on the kitchen table at 6am.

| | |
|---|---|
| **Built for** | Personal: my household |
| **Timeline** | One evening for the first full version (October 2026), with refinements since |
| **Status** | Live: a cloud job builds and publishes it every morning at 5:50am Mexico City time. Printing is built and paused until the printer has ink. |
| **Team** | Solo. I direct and decide; Claude Code writes the code. |
| **Stack** | Python, Jinja2, WeasyPrint, feedparser, pdftotext, Ollama, Gemma 4 (E4B on the laptop, 26B hosted), Qwen 3.5, OpenRouter, three banks' data (two REST APIs and PDF statements), systemd timers, CUPS, Claude Code scheduled tasks, claude.ai Artifacts |

---

## The problem

Our money is spread across three banks in three currencies: a US business bank, a multi-currency account with savings pots, and a Mexican bank that offers no API to personal customers. Seeing where things stood meant opening three apps, converting currencies in my head, and remembering which transfers were just money moving between our own accounts. I also follow AI news closely, and I wanted to try open-weight models I hadn't used before on a real job, not a toy prompt.

So the brief I set myself: every morning, a printed front page in the style of a classic broadsheet, with our balances in a stock-market box and the day's most important AI stories, with no manual steps.

## What I built

**Page one** is the front page: a blackletter masthead, a dateline, a lead story, six more stories in columns, and **The Markets**, a box with every chosen account converted to USD at the day's exchange rate, with a ▲/▼ change since yesterday. The bank with no API is read from its monthly PDF statement. The page labels that figure with the statement date, so it's never mistaken for a live balance.

**Page two** treats the household's accounts like a market page:

- **The Big Board:** each account is a ticker, with its opening balance for the month, its latest balance, the day's change, the share left, and a 30-day balance sparkline.
- **The monthly cycle:** the month starts when the main transfer goes out to the spending accounts, not on the 1st. Each account's opening balance is taken after its own funding lands.
- **The Sector Report:** spending by category, for example rent, groceries, subscriptions and care costs. The categories come from editable rules first, and the bank's own category as a fallback. The report compares each category with earlier cycles at the same point in the month.
- **The Transfers Desk:** pay, funding and moves between our own accounts are listed separately. That way they never count as spending, and nothing is counted twice.
- **A locked savings pot:** one savings pot must never be touched. Any withdrawal from it puts a black alarm banner on both pages.

**A company watch.** I work for amazee.io, which is part of Mirantis, which IREN recently bought. Gemma has a standing instruction to include one item about IREN whenever there's news, preferring real company news over stock-tip pieces. A Google News search provides the stories, and the item carries an "IREN Watch" label.

## The rules that keep a small model honest

The design rule I cared about most: **the model writes words, the code writes numbers.** Balances and changes go from the bank data straight into the template, and never pass through a language model. The model gets only the news feeds' own text, and cites stories by ID. The code, not the model, attaches the outlet name and link. Every number in a summary is checked against the stories it cites: if it isn't there, it gets a † on the page. Shorthand like "$15.3B" counts as a match for "$15.3 billion".

Other rules followed from that:

- **Read-only access:** read-only bank tokens and GET requests only.
- **Secrets stay put:** tokens never pass through the chat or logs.
- **Statements are checked:** every bank statement must add up. Opening balance plus deposits minus charges has to equal the closing balance, and the transaction counts have to match. If they don't, the page says "couldn't read statement" rather than print a wrong figure.
- **Failures don't stop the page:** if a feed or a bank fails, that section says "unavailable" and the rest still prints.

## Testing the models

I gave two open-weight models the same 25 real AI stories and asked each to choose a lead plus six more, merge stories covering the same event, and write summaries using only facts from the stories. Each ran twice, once with my first instructions and once after I'd tightened them, and I checked every summary by hand against its source.

| | Gemma 4 E4B | Qwen 3.5 4B |
|---|---|---|
| Factual errors (both runs) | None; a few slight overstatements | Four: "wants to" became "will"; an invented job title and pronoun; credited "analysts" the source never mentions; misread "before … could be patched" |
| Keeping "X says / claims" | Every time | Mostly, but added a source that doesn't exist |
| Merging same-event stories | Every time | Never |
| Time per edition | about 2 minutes | about 2 minutes |

Gemma won on accuracy, which matters most for a news page. The tests also changed the instructions. After Qwen turned an aspiration into a promise, I added a rule: keep the original's level of certainty, so "wants to" or "could" never becomes "will". The numbers check came from the same tests.

There was a hardware lesson too. My laptop's GPU has 4 GB of memory, of which about 2.9 GB was free. Gemma 4 E4B's download is 6.6 GB, so it never fit on the card, despite what the guides suggested. Ollama split it between GPU and CPU. That's slow but fine for a job that runs once a day.

When the job moved to the cloud, I switched to a larger Gemma 4 (26B) on OpenRouter's free tier. It wrote the same edition in 17 seconds instead of about 2.5 minutes, and was more faithful to its sources. The free pool was often rate-limited, so I added my own free Google AI Studio key through OpenRouter's bring-your-own-key option. The code also retries and falls back to another model if an answer comes back busy or unusable. The full comparison is written up in the project, along with how to re-run it on a new model.

## The plan to print it every morning

The original design was all local. A systemd timer starts the job at 5:50am, a few minutes before it's needed. The page is rendered to PDF with WeasyPrint and sent to our home laser printer with `lp`. Because the printer is black and white, the design is black and white too: blackletter masthead, serif body, rules between columns, ▲/▼ instead of red and green. I checked every layout change by rendering the PDF and confirming that each page still fits exactly on one US Letter sheet. WeasyPrint lays some things out differently from a browser, so a few layouts had to change.

Printing is paused for the most practical reason: the printer is out of ink. In the meantime the edition is published to a private claude.ai Artifact. When the printer is back, the laptop timer comes back alongside the cloud job, to do the printing.

## Moving it to the cloud

I wanted the page ready even when my laptop is off. It now runs as a Claude Code scheduled task: a cloud agent clones the repo at 5:50am, runs the edition, and republishes the Artifact. Getting there taught me a few things:

- **Keeping keys out of code:** the bank keys are stored as network secrets, which a proxy adds to outgoing requests, so they never appear in the code or the environment variables.
- **No persistence between runs:** the cloud keeps nothing from one morning to the next, so I stopped relying on yesterday's snapshot. The day's change is now worked out from each account's transaction history. That also means exchange-rate drift no longer shows up as a change.
- **Statements live in the repo:** the PDFs are committed to the private repository, which is the only way the cloud job can read them.
- **Test runs:** it took four test runs to get a clean edition. The failures were missing Python libraries, an output folder that doesn't exist in a fresh checkout, and a free model that once answered without any JSON. Each failure published nothing, which is the behaviour I wanted.

## What I'd do differently

- **Separate work and personal accounts from the start.** The first cloud version ran on my work Claude account. I checked what organisation admins can see, and the plan is to move it to a personal account.
- **Wait for history before judging "typical".** We switched bank accounts mid-month, so the "typical spending" comparisons have very little history behind them so far. They'll mean more after a few cycles.
- **Keep checking the copy.** Even the better model still slips occasionally. It once read "ran for two months before the vulnerability could be patched" as "the vulnerability could be patched". Every story links to its source for that reason.

## Where it stands now

Live. Every morning at about 5:52am a fresh two-page edition is waiting, and an open-weight model has written every word of the news. Next comes printing, once there's ink. After that, more months of history for the spending comparisons.
