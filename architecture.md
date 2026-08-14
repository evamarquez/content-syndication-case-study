# Architecture: content syndication programme

This document expands the public case study without reproducing platform credentials, account details, or source content.

## System model

```text
                  +------------------+
                  |  Source video    |
                  +--------+---------+
                           |
                           v
                  +--------+---------+
                  | Transcript and   |
                  | source context   |
                  +--------+---------+
                           |
                           v
                  +--------+---------+
                  | Master content   |
                  +---+----+----+----+
                      |    |    |
                      v    v    v
                 platform-specific variants
                      |    |    |
              +-------+    |    +--------+
              v            v             v
          direct API     RSS feed     manual queue
              |            |             |
              +------------+-------------+
                           v
                  +--------+---------+
                  | Scheduler and    |
                  | publish state    |
                  +--------+---------+
                           |
                           v
                  +--------+---------+
                  | Live checks and  |
                  | measurement      |
                  +------------------+
```

## Connector policy

The programme classified platforms by what they could safely support:

- Direct API publishing where the API and rate limits were reliable.
- RSS-based distribution where the platform supported a stable importer.
- Manual queues where no dependable publishing API existed.
- Exclusion or reduced priority where the platform could not provide useful reach or crawlability.

This prevented the scheduler from claiming full automation when the platform boundary made that impossible.

## Measurement model

The reporting layer treated these as different questions:

| Question | Evidence needed |
|---|---|
| Did the system publish? | Scheduler state plus live-page check |
| Did a person click? | Analytics that filters known crawler activity |
| Did the pages earn backlinks? | A crawler with coverage of the target pages |
| Can search or AI systems read the pages? | Robots, noindex, canonical, and live-fetch checks |
| Did the database count correctly? | Reconciliation against a public feed |

The distinction is the core design decision. One click counter or one backlink report cannot answer all five questions.

## Reliability controls

- Per-topic concurrency control.
- Retry-failed and deduplication paths.
- Per-platform rendering through a shared renderer.
- Truncation detection before content was saved or published.
- Live checks for platform URLs.
- A scheduled report that stated which sources had incomplete coverage.

