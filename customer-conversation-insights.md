# Customer Conversation Insights

> A scheduled Notion AI pipeline that reads recorded sales and customer meetings and turns them into two living databases: the questions prospects keep asking, with suggested answers for the technical account team, and ready-to-assign content briefs for Marketing.

| | |
|---|---|
| **Built for** | amazee.io (Marketing, Technical Account Managers and Customer Engagement Managers) |
| **Timeline** | January – March 2026 |
| **Status** | Delivered; ran on a schedule |
| **Team** | Solo build, designed with Marketing |
| **Stack** | Zoom Revenue Accelerator transcripts, Notion databases, Notion AI (scheduled prompt), Notion permissions; n8n and Zoom webhooks for automated transcript intake (tested, not yet live) |

---

## The problem

Two teams needed the same thing from customer conversations, and neither could get it.

Marketing was struggling to find recurring topics for content. They knew the best material was in what prospects actually asked on calls, but they weren't on those calls, and nobody had time to rewatch hours of recordings.

The Technical Account Managers (TAMs) and Customer Engagement Managers (CEMs) had the reverse problem. They were on the calls, but each one only saw their own accounts. If three prospects asked the same security question in the same month, nobody noticed the pattern, and each TAM wrote their answer from scratch.

The raw material already existed: amazee.io records its sales and customer meetings in Zoom, and the transcripts were piling up unread.

## What I built

**1. Transcripts into Notion.** Sales reps download each meeting's transcript from Zoom Revenue Accelerator and add it as a row in a *Sales Zoom Recording Transcripts* database, with the meeting title, date, account (linked to the company database), host, and the transcript file. The transcript arrived as an attached file, which Notion AI can't read. So I used Notion AI to put each transcript's text on the meeting's page itself, where it could be read.

**2. Tight permissions.** Customer conversations are sensitive. The transcripts database is limited to the people who need it, and the AI's job is to pull out patterns, not pass raw conversations around. The two output databases hold summaries, short quotes and links back to the source meeting, so access to the full transcript stays where it was.

**3. A fixed prompt on a schedule.** Notion AI runs the same prompt on a schedule over new transcripts and sorts what it finds into two databases.

***Prospect Meeting Insights*** is for the TAMs and CEMs. Each row is a recurring question or concern, with:

- the question in plain words, and a summary of what people actually mean by it
- topic, theme (security, pricing, compliance, features, performance, migration), funnel stage and intent (question, requirement or objection)
- the technologies involved (AWS, Kubernetes, CDN/WAF, Drupal and others)
- how often it came up, and a priority
- direct quotes of the question being asked, and of how our team answered it
- recommended messaging: talking points for the next time it comes up
- links back to the transcripts as evidence

***Marketing Content – Draft Briefs*** is for Marketing. Each row is a content idea that comes from those questions, with:

- a working title and format (blog post, FAQ, sales one-pager, webinar, social post series)
- primary and secondary topics
- target persona (technical user, developer, decision maker, enterprise buyer, influencer)
- the key points the piece has to make
- a call to action
- an owner and status, so Marketing can pick it up and run

## What it surfaced

From the first runs, the insights database held 16 recurring questions, half of them high priority. Fifteen came up during evaluation, and seven during procurement: the questions that decide deals. Security was the biggest theme. The recurring ones:

- What security and compliance posture (SOC 2, ISO 27001) do we inherit, and what stays our responsibility?
- Can amazee.io run in our own cloud account instead of an amazee-managed one?
- Can we install our own security agents without breaking your automation?
- Where do logs go, and can we send them to our SIEM?
- How do we estimate CDN, WAF and cloud costs so we're not surprised later?

The brief database turned those into 10 draft briefs: four FAQs, two blog posts, two sales one-pagers, a webinar and a social post series. One example: a blog post on what SOC 2 and ISO 27001 do (and don't) cover when the platform runs in the customer's own cloud account, aimed at decision makers going through vendor risk review.

## How I approached it

The design choice that mattered most was the fixed prompt with fixed fields. A free-form "summarise these calls" gives a different shape of answer every week, and you can't filter, count or trend it. Forcing every insight into the same fields (theme, funnel stage, intent, frequency, priority) is what lets Marketing sort by frequency and TAMs filter by technology.

Evidence links were the other deliberate choice. Every insight points back to the meetings it came from, and quotes are kept word for word, so anyone can check that the AI isn't inventing a trend. That's what makes the output something a TAM can trust in front of a customer.

I also kept it inside Notion on purpose. Marketing and the account teams already worked there, the permission model was already set up, and Notion AI could run on a schedule without a separate service to host or secure.

## Where it stands now

The pipeline ran on a schedule, feeding Marketing a steady list of topics taken from real customer questions, and giving TAMs and CEMs ready talking points for the questions they'd hear next. It's also one item on Marketing's [automation wishlist](./workstream-automation-wishlists.md) ("create a content to-do list from meeting transcripts") that went from idea to working system.

Getting transcripts in is still done by hand, but the automation is built. I first tried pulling conversations from the Zoom Revenue Accelerator API in n8n. The version that works listens instead: when Zoom finishes a recording, it sends a webhook to an n8n workflow. The workflow answers Zoom's URL validation check, waits for the files to be ready, picks out the transcript file, strips the timestamps, and creates the meeting's page in Notion with clean text. That also removes the step where Notion AI moves the transcript onto the page. It has been tested against a separate Zoom account, and goes live once it's connected to amazee.io's own Zoom.

The other next step is closing the loop: tagging published content against the question it answers, so Marketing can see which recurring questions still have nothing written for them.
