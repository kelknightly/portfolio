# Urban Resilience Sentinel

> A longitudinal data-archiving engine designed to capture the ephemeral urban data that disappears within 48 hours of a disruption — building the dataset that urban resilience researchers desperately need but can't currently get.

| | |
|---|---|
| **Built for** | Personal — research project |
| **Timeline** | A few hours so far; ongoing |
| **Status** | In progress |
| **Team** | Solo |
| **Stack** | Python, PostgreSQL, scheduled data collectors, multi-source APIs |

---

## The problem

When a city is disrupted — by flooding, extreme heat, a major earthquake, a supply chain shock — what actually happens to it? How quickly do transit systems degrade? Which supply chains break first? How long does it take businesses in affected areas to close, and then reopen?

These questions matter enormously for urban resilience research and city planning. But there's a fundamental data problem: the most valuable signals are ephemeral. A business's "temporarily closed" status on Google Maps disappears when it reopens. Transit disruption alerts expire. Delivery availability windows reset. The data that would tell you how a city functions under stress simply isn't being archived at the resolution or frequency needed to study it properly.

Existing research in this space is largely retrospective — it relies on surveys, incident reports, and insurance claims gathered weeks or months after an event. By then, the fine-grained operational data is gone.

## What I'm building

The Urban Resilience Sentinel (URS) is a data archiving engine that captures this data continuously, before any specific event occurs, creating a baseline against which disruption can be measured. It covers two cities — Vancouver, BC and Mexico City (CDMX) — across five data pillars:

- **Transit disruptions** — pulled from TransLink and CDMX GTFS-Realtime feeds every five minutes
- **Supply chain availability** — daily snapshots of delivery platform availability (Instacart, Uber Eats, Rappi, Walmart MX)
- **Business vitality** — weekly status snapshots from Google Places and local equivalents
- **Road infrastructure** — real-time feeds from DriveBC, OVIAL CDMX, and Waze
- **Urban flooding** — event-triggered data from City of Vancouver 311 and SACMEX rainfall sensors

The idea is simple: if you're archiving at high frequency before anything happens, you have the baseline to measure against when something does. The dataset that doesn't exist yet gets built over time, quietly, whether or not a disaster occurs.

## Why Vancouver and Mexico City?

Vancouver is a city with significant seismic risk, periodic atmospheric river flooding events, and a transit system that serves as a genuine lifeline for much of its population. Mexico City is one of the most seismically active major cities in the world and has faced flooding, supply chain disruption, and infrastructure stress repeatedly in recent decades.

Both cities also have reasonably accessible open data APIs — a prerequisite for this kind of continuous archiving.

## How I built it

Started with a few focused hours: schema design, the core data collection framework, and the first two data pillar collectors. This is a project where getting the data model right at the beginning matters more than building fast — a poorly structured archive is difficult to query and hard to correct retroactively.

Built in Python, with PostgreSQL for storage. The architecture is straightforward: scheduled jobs pull from each data source, normalise the data into a consistent schema, and store it with timestamps precise enough to reconstruct the sequence of events during a disruption.

## Where it stands now

Early stage. The foundation is built; the data collection framework is running. This is a slow-burn project — the value compounds over time as the dataset grows, not through rapid feature development.

The longer-term intention is to use this as part of a PhD application. The question I'm interested in is whether high-frequency, multi-pillar urban operational data — gathered before an event — can meaningfully improve our ability to model and predict functional failure and recovery latency after one. That's a research question as much as an engineering one, and the dataset URS is building is the prerequisite for asking it rigorously.

This project is different from the others in this portfolio. It's not solving an immediate business problem. It's building something with a longer time horizon and a different kind of purpose. I think that's worth including.
