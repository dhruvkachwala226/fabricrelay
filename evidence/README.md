# FabricRelay Codex evidence

This pack records the work performed for the Build with Codex One-Month Challenge. [M00](milestones/M00.md) documents repository initialization. [M01](milestones/M01.md) documents problem discovery and the proposed MVP, with customer validation pending. [M02](milestones/M02.md) documents the approved architecture and implementation plan. [M03–M14](../PLANS.md) remain pending.

## Evidence locations

| Record | Purpose |
| --- | --- |
| [Milestones](milestones/) | Individual records with the required evidence fields and completion status |
| [Prompts](prompts/) | Original requests for [M00](prompts/M00.md), [M01](prompts/M01.md) and [M02](prompts/M02.md) |
| [Screenshots](screenshots/) | Milestone folders for captures that substantiate work |
| [Logs](logs/) | Sanitized command or verification transcripts when retained |
| [Model usage](model-usage.csv) | Actual model, reasoning effort, selection reason, capabilities, and result |
| [Capabilities](capabilities.md) | Codex capabilities actually used and the work they supported |
| [Human decisions](human-decisions.md) | User-provided scope, decisions, and feedback |

## Lifecycle

1. [Understand](01-understand.md)
2. [Plan](02-plan.md)
3. [Build](03-build.md)
4. [Test](04-test.md)
5. [Review](05-review.md)
6. [Document](06-document.md)
7. [Ship](07-ship.md)
8. [Automate](08-automate.md)

## Recording standards

- Update the milestone after meaningful work. Use the actual local date in `YYYY-MM-DD`, cite the original prompt, and record the work performed, commands, outcomes, decisions, and limitations.
- Report the actual model, reasoning effort, and selection reason only when verified. Enter `Not Recorded` when session or historical metadata is unavailable. Use `Pending` for future milestones and explain absent artifacts or checks when the reason is known.
- Link only to artifacts that exist. Include a screenshot or log when it substantiates an outcome and can be safely shared. Redact credentials and private customer data from prompts, screenshots, logs, and other evidence before publishing. Document any evidence redaction.
- Record real commit references only after approved Git activity. Use completion status to reflect what was verified and what remains.
