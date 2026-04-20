# HubSpot Instance Merger — Amazee Labs → amazee.io

> A one-time CRM migration consolidating Contact and Company data from the Amazee Labs HubSpot instance into the amazee.io (AIO) instance — without disrupting existing automations, inflating the marketing contact count, or losing historical context.

| | |
|---|---|
| **Built for** | amazee.io |
| **Completed** | December 18, 2025 |
| **Status** | Delivered |
| **Type** | Data migration and CRM consolidation |
| **Systems** | HubSpot (Amazee Labs instance → amazee.io instance) |

---

## The problem

Amazee Labs and amazee.io were running separate HubSpot instances. As the two entities consolidated, their CRM data needed to merge — but not naively. The amazee.io HubSpot instance has live automation workflows, lead-scoring logic, and marketing sequences that could fire incorrectly if foreign records landed without the right properties. A simple import would have created duplicates, broken attribution, triggered unwanted email sends, and inflated the marketing contact billing count.

The challenge was getting the data across cleanly: with correct associations, mapped lifecycle stages, appropriate marketing status, and without overwriting or corrupting anything already in AIO.

## What I did

The migration covered Contacts and Companies. Before importing anything, I audited both export files thoroughly — identifying empty columns, read-only system properties that can't be imported, properties with insufficient data density to be worth carrying over, and properties that existed in Labs but had no equivalent in AIO.

Columns with no data or fewer than ten records were stripped from the import files before they touched HubSpot. Read-only system analytics properties (create dates, last activity, source data, traffic properties, owner assignment timestamps) were documented and excluded — HubSpot manages these natively and they can't be written via import regardless. This left clean, minimal import files that mapped only to fields that could actually be written.

**Lifecycle stage mapping** was one of the more consequential decisions. Labs used different lifecycle stage labels than AIO. Mapping them incorrectly would have enrolled contacts into the wrong HubSpot workflows. I reviewed the Labs automations to understand what each stage represented in practice, then mapped them to AIO's numerical standard:

| Labs value | AIO value |
|---|---|
| Blanks, Subscriber, Lead | 1 - Identified |
| Opportunity | 4 - In Deal |
| Customer | 5 - Active Customer |
| Former Customer | 6 - Churned/Inactive |

**Association directionality** was a technical issue that required a fix mid-migration. The initial import was only establishing Company-to-Contact associations in one direction. I corrected this with a multi-file import using Associated Company as the shared unique key to ensure proper bidirectional Contact-to-Company linking.

**Marketing contact status** was deliberately not set during import. Marking migrated contacts as marketing contacts would have caused an immediate billing spike and potentially triggered nurture sequences they hadn't consented to in the AIO context. Instead, I preserved a custom property (`Amazee Labs Marketing Contact Status`) from the Labs export so the marketing team can identify and segment these contacts for Labs-specific campaigns separately, on their own timeline.

Custom properties that existed in Labs but had no clear equivalent in AIO were evaluated individually. Several were flagged for review with the relevant stakeholder (Gesche) before being discarded — this was documented rather than assumed.

## What couldn't be migrated

Some things are simply not possible via CSV import in HubSpot. Historical activity logs — meetings, calls, emails — can't be re-associated with contact records this way. The mapping interface doesn't support activity associations. This was documented upfront so there were no surprises after the fact. The migrated records are identifiable in AIO via `Record Source Detail 1` filtering on the import filename (`18-Dec-2025-Amazee-Labs-Contacts.csv`), which makes it easy to scope any follow-up work.

## Where it stands now

The migration is complete. Contacts and Companies from Amazee Labs are in the amazee.io HubSpot instance, correctly associated, with lifecycle stages mapped to AIO standards, and without having triggered any downstream automations or inflated billing. Historical activity is absent by necessity, but the current state of each record is intact.

This kind of project isn't glamorous — it's mostly careful decisions about what to include, what to exclude, and in what order. But getting it wrong would have caused real problems for the sales team, and getting it right required understanding how HubSpot's automation layer works well enough to anticipate every place a bad import could cause downstream damage.
