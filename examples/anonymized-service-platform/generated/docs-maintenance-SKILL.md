---
name: docs-maintenance
description: "Use branch diffs or other change evidence plus docs/intent to decide whether documented areas need a content update, verification refresh, new doc, or no change."
---

# Documentation Maintenance Skill

This illustrative generated artifact shows what the split maintenance skill can look like after bindings are resolved for the anonymized service platform.

## Porting Metadata

Ported from RepoBrain Skill Blueprint
Blueprint version: 2026.11.25
Blueprint source ref: example-blueprint-ref
Ported on: 2026-11-25

## Goal

Keep `docs/intent/` aligned with meaningful changes in `services/api/src/`, `services/worker/src/`, `packages/shared/`, and `config/` without turning docs into implementation inventories.

## Load First

- `AGENTS.md`
- `docs/system-overview.md`
- `docs/intent/_template.md`
- the available change evidence
- `.agent/notes/docs-skill-port.md` if present

## Evidence Source Order

1. Explicit user-supplied review target or diff range.
2. `git diff origin/main...HEAD`.
3. CI changed-file manifest.
4. Patch excerpt attached to a review task.
5. Manually curated component list, with blind spots stated explicitly.

## Procedure

1. Establish the strongest available evidence source before reasoning about doc freshness.
2. Map changed files to documented areas under `docs/intent/`.
3. If the change starts in `packages/shared/` or `config/`, trace actual consumers in `services/api/src/` and `services/worker/src/` before touching downstream docs.
4. Choose exactly one action per documented area: content update, verification-only refresh, no change, or new doc.
5. Update discovery docs such as `docs/intent/index.md` or `docs/system-overview.md` only when names, boundaries, or paths changed.

## Output Contract

State:

- the evidence source used
- the diff scope used
- the documented areas reviewed
- the action chosen for each area
- any blind spots or unresolved impact questions

## Validation

Before finishing, confirm that:

- no literal placeholder markers remain
- the skill format still matches the destination runtime, and the front matter passes a format check
- the evidence source used was stated explicitly
- blind spots were called out when evidence was partial
- shared-code and config changes were traced into real consumers before downstream docs were updated
