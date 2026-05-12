# <SERVICE_OR_WORKFLOW_NAME>

This illustrative file shows the canonical intent-doc shape chosen for the anonymized service platform during bootstrap.

## Purpose

One or two sentences describing what this area is responsible for and what it is not responsible for.

## Entry Points

- Main inbound route, queue consumer, job handler, or command.
- Primary code locations another agent should inspect first.

## Key Rules

- Non-obvious priority, fallback, retry, or state-transition rules.
- Boundaries that keep this doc focused on one meaningful workflow.

## Dependencies

- Important upstream inputs.
- Important downstream effects.
- Shared config or shared libraries that materially change behavior.

## Verification Notes

- Code locations reviewed while writing the doc.
- Known drift, open questions, or intentional omissions.
- Review date if the destination repository tracks one.
