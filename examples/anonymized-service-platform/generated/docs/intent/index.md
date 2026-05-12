# Intent Docs Index

This bootstrap index gives the orientation skill one stable discovery path before the repository has full documentation coverage.

## How To Use This Folder

- Read `AGENTS.md` and `docs/system-overview.md` before treating any intent doc as authoritative.
- Start with the narrowest doc that matches the current question.
- Verify behavior in code before making claims about current runtime truth.

## Seed Docs

- `orders.md`: API-side order intake, validation, persistence, and async handoff into downstream processing.
- `notifications.md`: worker-side notification delivery, retry behavior, and provider boundary decisions.

## Current Coverage Boundary

This is an intentionally small bootstrap baseline, not a full system map.

Add another intent doc only when an area becomes a frequent orientation target or a meaningful maintenance-review surface.
