# Quality and measurement controls

## Control register

| Risk | Control |
|---|---|
| Truncated content | Reject incomplete output before saving or publishing. |
| Duplicate posts | Use deduplication and stable topic-level identifiers. |
| Render drift | Use one shared renderer and verify representative output. |
| API false positives | Check publication state and the live URL after posting. |
| Unsupported automation | Route the platform to a manual queue. |
| Bot activity | Filter known crawler patterns before interpreting clicks. |
| Weak claim evidence | Report missing crawl coverage, robots blocks, and noindex states. |

## Measurement model

The report answered four separate questions:

| Question | Evidence |
|---|---|
| Did the system publish? | Scheduler state plus live-page check |
| Did a person click? | Analytics with known crawler activity filtered out |
| Did pages earn backlinks? | Crawler coverage of target pages |
| Can search or AI systems read them? | Robots, noindex, canonical, and live-fetch checks |

## Human judgment

The system could prepare, publish, and inspect content. It could not decide that a claim was supported, that a channel created useful reach, or that a client rollout was justified. Those decisions remained with the operator and editorial review.
