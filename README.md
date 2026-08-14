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

## Goal and implementation plan

The goal was to build a measured publishing programme without scaling an unproven content or distribution model to clients.

The implementation plan was deliberately staged:

1. Validate the strategy cheaply with a small number of branded platforms.
2. Separate the content engine from the scheduling and publishing tool.
3. Automate reliable API and RSS paths while keeping unsupported platforms in a manual queue.
4. Run a New Seas pilot with live checks and measurement before considering client rollout.

That sequence kept the decision reversible. The programme could prove or disprove its value before a larger multi-client build.

## What I built

- A content-engine handoff from source video and transcript to platform-specific versions.
- A scheduler and publishing layer for roughly twelve Web2 platforms.
- Direct platform connectors where APIs existed and RSS-based paths where they were more reliable.
- Manual queues for platforms that could not support safe automated publishing.
- Retry, deduplication, concurrency control, and per-topic visibility into batch generation.
- A shared renderer so headings, lists, links, and platform-specific formats stayed consistent.
- Live checks that verified whether a published URL actually existed and remained reachable.
- A reporting layer combining scheduler data with crawler checks and external measurement sources.
- A quality and AI-readability audit that checked what was actually reachable by search and AI crawlers.

## Architecture and production flow

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

### Production and reliability

#### One source, distinct platform versions

The system generated a platform-specific version rather than copying the same article everywhere. This reduced measured duplication from 62.6% to 25.1% and made the distribution programme more defensible as content work rather than mass reposting.

#### Automate where the platform allows it

The publishing matrix separated direct APIs, RSS imports, and manual queues. Some platforms did not support reliable automated publishing. Those stayed manual or were treated as lower-value channels instead of being represented as fully automated.

#### Reliability controls

- Per-topic concurrency control for large batches.
- Retry-failed and deduplication paths.
- One shared renderer instead of separate markdown-to-HTML implementations.
- Truncation detection before content was saved or published.
- Live checks for platform URLs.
- A scheduled report that stated which sources had incomplete coverage.

## Results

- The programme was built from zero to a working publishing system in one month.
- It produced 352 articles and 527,369 words across roughly twelve platforms in a month.
- Cross-platform duplication decreased from 62.6% to 25.1%.
- The weekly report runtime dropped from 132 seconds to 5 seconds.
- The audit identified 79 articles blocked by robots rules and noindex directives.

## Failure analysis and lessons learned

The system became valuable because it exposed its own failure modes instead of hiding them behind a volume number.

### Truncated content was being saved as complete

The generator could run out of tokens and save a post that ended in the middle of a sentence. The publishing path was changed to reject incomplete output rather than store it as successful content. A repair path was added for affected posts.

### One platform received raw markdown

One connector published headings and links as plain text. Three copies of the renderer had drifted apart, so the fix was to centralize rendering and verify the output before publishing. The platform-specific repair pass removed the raw markdown from the affected posts.

### Published did not always mean live

Some posts returned an ID from a platform API but remained drafts. The system had been trusting the request response instead of checking the returned publication state. The reporting model was changed to distinguish created, published, reachable, and manually pending.

### Redirects created the wrong measurement

The first link path sent readers through a click tracker before reaching the site. That made the tracker look like the backlink destination and could not prove authority to the target domain. The publishing path was corrected to use direct site links with intentional tracking parameters.

### The content audit found unsupported claims

An editorial audit found fabricated or unsupported statistics, outdated advice, and claims attached to sources that did not support them. A correction pass removed 250 unsupported claims from 663 stored posts. The lesson was more important than the cleanup: verifying that a URL exists is not the same as verifying that the claim is true.

### Database counts were not enough

The scheduler database was missing 51 published articles and counted 29 duplicates as unique entries. Public feeds were used as a second source before reporting the final totals.

## Measurement and what we could not claim

The reporting layer separated four different questions:

| Question | Evidence used |
|---|---|
| Did the system publish? | Scheduler state plus live-page check |
| Did a person click? | Analytics with known crawler activity filtered out |
| Did the pages earn backlinks? | A crawler with coverage of the target pages |
| Can search or AI systems read the pages? | Robots, noindex, canonical, and live-fetch checks |

The audit found that the raw click total included a bot burst, the chosen backlink source had no crawl coverage for the free subdomains, and the baseline AI-visibility check returned zero citations. Those were reported as measurement limits, not converted into positive results.

## Security and operational hardening

- Platform credentials and account details remain outside the public case study.
- Publishing paths use scoped connectors and explicit manual boundaries.
- Content is checked for truncation and render integrity before live publishing.
- Failed and duplicate jobs are visible rather than silently retried into additional posts.
- Platform limitations are documented so a manual queue is not mistaken for automation.

## Handoff and post-launch operations

- The reporting layer was redesigned into a simple all-platform view and focused per-channel views.
- App statistics were separated from Bing and AI-visibility measurements.
- An AI-readability audit established a baseline for monthly comparison.
- The report records source coverage and measurement limitations alongside the numbers.
- The programme conclusion stayed deliberately narrow: it had production volume and a working distribution layer, but it could not yet prove human traffic, backlinks, or AI citations without better instrumentation.

## What I owned

I owned the programme design, platform matrix, content-generation handoff, scheduler architecture, publishing reliability work, reporting logic, and the audit that separated evidence from assumptions.

## Privacy boundary

See [privacy.md](privacy.md) for the exact boundary around what is intentionally omitted.
