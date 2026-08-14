# Content syndication programme

How I built a multi-platform publishing system from zero in one month, then audited the numbers before calling it a success.

This is a sanitized case study. Platform accounts, client names, URLs, credentials, source content, and publishing data are not published.

## At a glance

| | |
|---|---|
| Role | AI Engineer and programme builder |
| Build time | From plan to working programme in June 2026 |
| Distribution | Roughly 12 Web2 platforms |
| Output | 352 articles and 527,369 words in one month |
| Distinctness | Cross-platform duplication reduced from 62.6% to 25.1% |
| Reporting | Weekly report reduced from 132 seconds to 5 seconds |

## The problem

The programme needed to turn one source video into distinct, branded versions for multiple publishing platforms. It also needed a scheduler, publishing connectors, a review path, and a report that could answer whether the programme was creating useful distribution.

The first version of the plan assumed that volume and a click counter would be enough. Building the system exposed a harder problem: platform publishing data is not automatically evidence of human reach, backlinks, or AI visibility.

## What I built

- A content-engine handoff from source video and transcript to platform-specific versions.
- A scheduler and publishing layer for roughly twelve Web2 platforms.
- Direct platform connectors where APIs existed and RSS-based paths where they were more reliable.
- Manual queues for platforms that could not support safe automated publishing.
- Retry, deduplication, concurrency control, and per-topic visibility into batch generation.
- A reporting layer combining scheduler data with crawler checks and external measurement sources.
- A quality and AI-readability audit that checked what was actually reachable by search and AI crawlers.

## Architecture

```text
source video
     |
     v
transcript + source context
     |
     v
master content -> platform-specific variants
                         |
                         v
                 review and scheduling
                  /        |        \
                 v         v         v
             API posts   RSS feeds  manual queue
                 \         |         /
                  v        v        v
                 platform publishing
                         |
                         v
             live checks + reporting
```

The expanded model is in [architecture.md](architecture.md).

## Design decisions

### One source, distinct platform versions

The system generated a platform-specific version rather than copying the same article everywhere. This reduced measured duplication from 62.6% to 25.1% and made the distribution programme more defensible as content work rather than mass reposting.

### Automate where the platform allows it

The publishing matrix separated direct APIs, RSS imports, and manual queues. Some platforms did not support reliable automated publishing. Those stayed manual or were treated as lower-value channels instead of being represented as fully automated.

### Measure reach with sources that can answer the question

The report separated click activity, crawlability, backlinks, and AI visibility. A single counter cannot answer all four. Each metric received a source and a limitation statement.

### Audit the system itself

The reporting layer checked live pages, robots directives, canonical URLs, database counts, and crawler behaviour. This made the programme capable of reporting that a measurement was unavailable, rather than filling the gap with a flattering number.

## Results

- The programme was built from zero to a working publishing system in one month.
- It produced 352 articles and 527,369 words across roughly twelve platforms in a month.
- Cross-platform duplication decreased from 62.6% to 25.1%.
- The weekly report runtime dropped from 132 seconds to 5 seconds.
- The audit identified 79 articles blocked by robots rules and noindex directives.

## Three numbers that could not be trusted

### 1. The click total included a crawler

The tracker recorded 836 clicks. 546 came from one bot in a short burst. The raw total could not be presented as human traffic, so the report filtered the burst and stated that it had done so.

### 2. The backlink count was not measurable through the chosen source

Ahrefs had crawled zero pages on the free subdomains used by the programme. A backlink count of zero from that source did not prove that the programme had created zero backlinks. It proved that the source had no crawl coverage for this measurement. The correct result was an explicit limitation, not a made-up trend line.

### 3. The scheduler database count was incomplete

The database was missing 51 published articles and counted 29 duplicates as unique entries. Counts taken from the database alone were therefore wrong in both directions. The public feed was used as a cross-check before reporting totals.

## What the audit changed

- Bot bursts were separated from click totals.
- Live pages were fetched as a crawler to detect robots and noindex blocks.
- Public feeds were used to reconcile missing or duplicated database entries.
- The report separated app statistics from backlink and AI-visibility claims.
- Platform-specific rendering was centralized so headings, lists, and links did not degrade differently by connector.
- Truncated output was rejected instead of being saved as complete content.

The conclusion was deliberately narrower than the original ambition: the programme had production volume and a working distribution layer, but its first measurement model could not prove human traffic, backlinks, or AI citations without better instrumentation.

## What I owned

I owned the programme design, platform matrix, content-generation handoff, scheduler architecture, publishing reliability work, reporting logic, and the audit that separated evidence from assumptions.

## Privacy boundary

See [privacy.md](privacy.md) for the exact boundary around what is intentionally omitted.

