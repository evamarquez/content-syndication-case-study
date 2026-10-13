# Production evidence

## Measured results

| Measure | Result |
|---|---|
| Platforms | Roughly 12 Web2 platforms |
| Articles | 352 in one month |
| Words | 527,369 |
| Cross-platform duplication | Reduced from 62.6% to 25.1% |
| Weekly report runtime | Reduced from 132 seconds to 5 seconds |
| Audit finding | 79 articles blocked by robots rules or noindex directives |

## Reliability findings

- Truncated content was rejected instead of being stored as complete.
- A raw-markdown connector was repaired through the shared renderer.
- Posts that returned an API ID but stayed drafts were separated from live publications.
- Direct site links replaced a redirect path that could not prove authority to the target domain.
- A correction pass removed 250 unsupported claims from 663 stored posts.
- Public feeds exposed 51 published articles missing from the scheduler database and 29 duplicates counted as unique.

## What the evidence supports

The evidence supports a conclusion about production throughput, publishing reliability, duplication control, reporting speed, and measurement discipline.

## What it does not support

The results do not prove human traffic, backlinks, search ranking, revenue, or AI citations. The audit found missing crawl coverage and a zero-citation baseline, so those remained open questions.
