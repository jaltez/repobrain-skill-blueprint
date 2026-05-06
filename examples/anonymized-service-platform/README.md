# Example Port: Anonymized Service Platform

This is an anonymized example of how a coding agent could port this blueprint into a real destination repository.

It shows three things:

- the local placeholder bindings chosen during the port
- the inspection-complete record used before template adaptation
- the bootstrap actions taken because the destination repository did not yet have an intent-doc system
- the final artifact set created in the destination repository
- concrete generated example artifacts produced from those decisions

## Destination Repository Profile

This example destination repository is intentionally generic.

- multi-package service platform with an API service, a background worker, and a shared library area
- repository-wide instructions already exist in `AGENTS.md`
- no intent-doc system existed before the port
- a lightweight system overview already existed but needed to be treated as the architecture overview source
- local branch review usually uses git history, but some CI review jobs expose only changed-file manifests

## Inspection Complete Record

Before adapting templates, the porting agent had identified or confirmed absent at least the following:

- repository-wide rules source: `AGENTS.md`
- architecture overview source: `docs/system-overview.md`
- intent-doc root state: absent before port, to be created under `docs/intent/`
- existing similar skills or documentation-memory systems: none found
- source roots: `services/api/src/` and `services/worker/src/`
- shared-code root: `packages/shared/`
- config root: `config/`
- maintenance evidence source order: explicit review target, `git diff origin/main...HEAD`, CI changed-file manifest, patch excerpt, manual component list
- runtime/schema assumption: the local skill runtime accepts YAML front matter with `name`, `description`, and `argument-hint`, followed by freeform markdown sections

This record existed before template adaptation started.

## Port Decisions

Chosen layout:

- two separate skills
- orientation skill first
- maintenance skill second

Chosen rollout:

1. keep the existing system overview as the architecture overview source
2. create a new intent-doc root and canonical doc template
3. seed the first two intent docs for the highest-value areas
4. generate the orientation skill and validate it
5. generate the maintenance skill once the documentation target is stable

Chosen maintenance evidence strategy:

1. explicit user-supplied review target or diff range
2. `git diff origin/main...HEAD`
3. CI-produced changed-file manifest
4. patch excerpt attached to a review task
5. manually curated component list with limitations called out explicitly

## Local Placeholder Bindings

- repository-wide rules source: `AGENTS.md`
- architecture overview source: `docs/system-overview.md`
- intent-doc root: `docs/intent/`
- source root: `services/api/src/` and `services/worker/src/`
- config root: `config/`
- shared-code root: `packages/shared/`
- canonical doc template: `docs/intent/_template.md`
- entrypoint search pattern: `main.ts|index.ts|router.ts|handler.ts`
- registration search pattern: `router\.|app\.use|queue\.process|register`
- config-access search pattern: `process\.env|getConfig\(|config\.`
- shared-dependency search pattern: `@shared/|packages/shared|from '@platform/shared'`
- diff-base fallback strategy: merge-base against `origin/main`

## Bootstrap Actions Taken

Because the repository had no intent-doc system yet, the porting agent created the smallest viable documentation target before generating the full skill pair.

This example shows the bootstrap path. In a repository that already has partial docs, the equivalent first pass could normalize existing docs into a stable intent-doc shape instead of creating them all net-new.

Bootstrap output:

- `docs/intent/`
- `docs/intent/_template.md`
- `docs/intent/index.md`
- `docs/intent/orders.md`
- `docs/intent/notifications.md`

For this example, the destination repository's `docs/intent/_template.md` could be derived from [templates/intent-doc-template-sketch.md](../../templates/intent-doc-template-sketch.md) and then localized to the platform's own terminology.

The orientation skill was generated and validated against those docs first. The maintenance skill was generated only after the new documentation target proved stable enough for repeated review.

## Final Artifact Set In The Destination Repository

After the port, the destination repository contained at least the following new or updated artifacts:

- `.github/skills/docs-orientation/SKILL.md`
- `.github/skills/docs-maintenance/SKILL.md`
- `docs/intent/_template.md`
- `docs/intent/index.md`
- `docs/intent/orders.md`
- `docs/intent/notifications.md`
- `.agent/notes/docs-skill-port.md`

The example intentionally includes both the generated skills and the supporting documentation artifacts that make those skills useful.

This blueprint repository also includes illustrative generated artifacts for this example port:

- `generated/docs-orientation-SKILL.md`
- `generated/docs-maintenance-SKILL.md`
- `generated/docs-skill-port-note.md`

## Example Provenance Marker

Stored near the top of each generated skill or in a repository note when inline metadata is not safe:

```text
Ported from RepoBrain Skill Blueprint
Blueprint version: 2026.05.06
Blueprint source ref: <tag-or-commit>
Ported on: 2026-05-05
```

## Example Validation Prompts

Orientation validation:

- `Orient me in order processing before I edit code.`
- `Use the local docs and current code to explain how notification delivery works.`

Maintenance validation:

- `Refresh the intent docs affected by the current branch.`
- `Given this changed file list, tell me which documented areas need review.`
- `Review this patch excerpt and decide which docs need updates.`

Validation should fail for this example if a generated artifact still contains literal `<...>` placeholders, cannot point to local docs or code paths, or omits the maintenance evidence source.

## Why This Example Matters

This example demonstrates the intended end state:

- the blueprint can be ported even when the destination repository starts without intent docs
- the local placeholder bindings are explicit and recoverable later
- the maintenance workflow is not blocked when only partial change evidence is available
- the final artifact set includes both generated skills and the supporting documentation structure they depend on
