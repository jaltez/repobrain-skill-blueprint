# Documentation Skill Port Note

This illustrative note shows the minimum record the porting agent created before adapting templates for the anonymized service platform.

## Provenance

- Blueprint: RepoBrain Skill Blueprint
- Blueprint version: 2026.11.25
- Blueprint source ref: example-blueprint-ref
- Port date: 2026-11-25

## Inspection Complete Record

- Repository-wide rules source: `AGENTS.md`
- Architecture overview source: `docs/system-overview.md`
- Intent-doc state before port: missing; target root chosen as `docs/intent/`
- Existing overlapping documentation skills or memory systems: none found
- Source roots: `services/api/src/`, `services/worker/src/`
- Shared-code root: `packages/shared/`
- Config root: `config/`
- Destination runtime/schema: the runtime follows the Agent Skills front matter contract; generated skills use `name` and `description` only, with no runtime-specific extension fields

## Bound Placeholders

- orientation skill name: `docs-orientation`
- maintenance skill name: `docs-maintenance`
- skill directory: `.agents/skills/docs-orientation/` and `.agents/skills/docs-maintenance/`, each named exactly like its `name` field
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

## Format Validation

- Both generated skills pass an Agent Skills format check: the directory name equals `name`, no non-standard front matter fields, and `description` within 1024 characters.
