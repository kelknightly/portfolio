# Mammoth Marketing Dashboard

> A marketing analytics dashboard that answers the question leadership actually asks: is our marketing turning into paying customers and leads the sales team can use? It pulls web traffic, conversions, ad spend and CRM data into one place, refreshed daily, with every number explaining where it came from.

| | |
|---|---|
| **Built for** | amazee.io |
| **Timeline** | May – July 2026 |
| **Status** | Live, behind company single sign-on, with a daily data refresh |
| **Team** | Solo, built with Claude |
| **Stack** | Python ETL, PostgreSQL with a SQL view layer, Next.js, TypeScript, BigQuery (GA4), Matomo API, HubSpot API, Google Ads API, Lagoon, Auth.js + Okta |

---

## The problem

amazee.io's marketing team was running campaigns, webinars, events and paid ads, and measuring success with soft signals: new contacts in HubSpot, website visits, webinar sign-ups. The growth lead needed something different: new contacts could be up to a thousand a day and it wouldn't matter. What mattered was how many turned into paying customers, and at what rate.

The two people who needed the dashboard were measuring different things. The marketing manager needed an operational view: is what I did this week having any effect? The growth lead needed a strategic one: is the spend producing revenue and pipeline? And the data to answer either question lived in four or five systems that disagreed with each other.

## What I built

The dashboard is split along the line that matters most in the business: amazee.io has two marketing motions that must never be blended.

- **Product-led growth (PLG):** the self-serve AI gateway. Someone clicks an ad, signs up, gets an API key and pays by card with no sales contact. Here the dashboard shows a funnel from spend to payment, with drop-off at each step, regional breakdowns, and cost per acquisition by channel.
- **Enterprise:** sales-led cloud and AI products with 6–18 month sales cycles. Here direct return on ad spend is meaningless, so the dashboard shows leads by source and lifecycle stage, pipeline value attributable to marketing, paid media by region and channel, and events.

A campaigns view shows what's in flight, what finished recently, budget against spend, and whether it worked. Everything filters by region (North America, Europe, APAC), because ad copy and performance differ by region and leadership wants to report that way.

Every stat tile, chart and rate column carries a plain-language tooltip that says what the number is, which system it comes from, and what it does *not* mean. "Conversions" is the most dangerous word in marketing data: a form submission, a scroll and a webinar registration are all "conversions" in Google Analytics with wildly different business value, so the dashboard always says which kind.

## How I built it

The data work came first, and it's most of the project. Raw data stays where it lives. Python ETL jobs pull what the dashboard needs from Matomo (web traffic), BigQuery (Google Analytics 4 conversion events), HubSpot (contacts, deals and seven pipelines) and the Google Ads API into Postgres as daily snapshots. The app reads SQL views, never raw tables, so a schema change upstream doesn't break the dashboard. Every ETL job is idempotent: it records each run and skips what's already loaded, so backfills are safe to repeat.

Several decisions came out of discovering how the sources actually behave, not how they were supposed to:

- **Matomo is the authority for traffic.** Google Analytics undercounted amazee.io traffic by about 4.4× because of consent sampling.
- **HubSpot wins on lead attribution.** It backfills a contact's source after creation, so when it disagrees with Google Analytics, HubSpot is right.
- **The data pipeline everyone assumed existed didn't.** The docs said Google Ads data flowed into BigQuery. I checked: nothing from Google Ads, Meta or Bing had ever landed there. I rerouted paid media to pull from the Google Ads API directly and documented the gap so nobody builds on the assumption again. Channel and region come from the campaign naming convention; region codes that don't match it are stored unmapped rather than guessed.

Everything is read-only against the source systems. The dashboard deploys on Lagoon with a scheduled daily ETL job, and sits behind Okta single sign-on. A monthly maintenance runbook lets an AI agent check dependencies, scheduled jobs and security, then produce a report and a pull request for a person to approve.

## Where it stands now

Live. The PLG, Enterprise and campaign views run on real data refreshed daily, with Enterprise paid media broken down by region and channel. The next phase is the data that's still missing: Stripe revenue (which unlocks true return on ad spend and customer lifetime value), Meta and Bing ads, and the product sign-up events that complete the PLG funnel.

This project is mostly invisible work. The interface is the easy part. What makes it trustworthy is knowing which system to believe for which number, and saying so on the page.
