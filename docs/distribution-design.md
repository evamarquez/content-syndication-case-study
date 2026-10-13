# Distribution design

## Delivery model

The system separated the content engine from scheduling, publishing, and measurement:

1. Source video and transcript
2. Master content and brand context
3. Platform-specific variants
4. Review and scheduling
5. API, RSS, or manual publishing
6. Live-page verification
7. Reporting and audit

## Platform matrix

| Path | When it was used | Boundary |
|---|---|---|
| Direct API | The platform exposed a reliable publishing interface. | Verify the returned state and live URL. |
| RSS import | The platform accepted a feed more reliably than direct publishing. | Confirm that imported content becomes reachable. |
| Manual queue | Safe automation was unavailable or unsupported. | Never represent it as fully automated. |

## Key decisions

### Distinct versions by platform

The system generated platform-specific versions instead of copying one article everywhere. This reduced measured cross-platform duplication and made the programme more defensible.

### One renderer

Three drifting markdown-to-HTML implementations were replaced by a shared renderer. A single output path made headings, lists, links, and platform formats easier to test.

### Publication as a verified state

The workflow distinguished created, published, reachable, and manually pending. An API response alone was not treated as proof that a reader could access the post.

### Keep the decision reversible

The programme was staged as a pilot. It could prove or disprove value before a larger multi-client build.
