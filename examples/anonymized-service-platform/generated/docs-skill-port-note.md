# Documentation Skill Port Note

This illustrative note shows the minimum record the porting agent created before adapting templates for the anonymized service platform.

## Provenance

- Blueprint: RepoBrain Skill Blueprint
- Blueprint version: 2026.05.06
- Blueprint source ref: example-blueprint-ref
- Port date: 2026-05-06

## Inspection Complete Record

- Repository-wide rules source: `AGENTS.md`
- Architecture overview source: `docs/system-overview.md`
- Intent-doc state before port: missing; target root chosen as `docs/intent/`
- Existing overlapping documentation skills or memory systems: none found
- Source roots: `services/api/src/`, `services/worker/src/`
- Shared-code root: `packages/shared/`
- Config root: `config/`
- Destination runtime/schema: YAML front matter with `name`, `description`, and `argument-hint` is accepted

## Bound Placeholders

- orientation skill name: `docs-orientation`
- maintenance skill name: `docs-maintenance`
- canonical doc template: `docs/intent/_template.md`
- entrypoint search pattern: `main.ts|index.ts|router.ts|handler.ts`
- registration search pattern: `router\.|app\.use|queue\.process|register`
- config-access search pattern: `process\.env|getConfig\(|config\.`
- shared-dependency search pattern: `@shared/|packages/shared|from '@platform/shared'`
- diff-base fallback strategy: merge-base against `origin/main`

## Maintenance Evidence Source Order

1. Explicit user-supplied review target or diff range.
2. `git diff origin/main...HEAD`.
3. CI changed-file manifest.
4. Patch excerpt attached to a review task.
5. Manually curated component list.

## Known Blind Spots

- Changed-file manifests can miss deleted or renamed docs unless the review artifact calls them out explicitly.
- Shared library impact still requires consumer tracing in the services before downstream docs are updated.
