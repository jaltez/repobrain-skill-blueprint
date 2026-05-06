# Intent Doc Template Sketch

Use this sketch when a destination repository does not yet have a usable intent-doc baseline, or when existing docs need a canonical shape to normalize toward during the first documentation pass.

This is not a required universal schema. Adapt the headings, metadata, and terminology to the destination repository's own conventions.

```text
# <COMPONENT_OR_WORKFLOW_NAME>

## Purpose
One or two sentences describing what this area is responsible for and what it is not responsible for.

## Entry Points
- Main inbound path, trigger, route, job, or command.
- Primary code locations another agent should inspect first.

## Key Rules
- Non-obvious priority, fallback, or state-transition rules.
- Caveats that materially affect reasoning or edits.

## Dependencies
- Important upstream inputs.
- Important downstream effects.
- Shared config or shared code that materially changes behavior.

## Verification Notes
- Code locations reviewed while writing this doc.
- Known drift, open questions, or uncertainty.
- Review date if the destination repository tracks one.
```

Minimum bootstrap recommendation:

- create one architecture overview or system map
- create one canonical intent doc template from this sketch
- create one or two seed docs for the highest-value areas first

Normalization-first variant:

- keep any existing docs that are already good enough
- adapt one representative doc into this canonical shape
- normalize one or two high-value existing docs before generating the orientation skill

Keep the first version concise. The goal is to create a stable documentation target that the orientation skill can use immediately, not to document the whole system at once.
