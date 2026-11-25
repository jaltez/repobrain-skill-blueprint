# Design Rationale

This repository is written for coding agents first and human readers second.

The goal is not to publish a universal skill package. The goal is to give a capable coding agent a reliable procedure for generating project-specific documentation skills that match a destination repository's own conventions.

## Primary Recommendation

Generate two separate skills in the destination repository.

- An orientation skill that reads intent-focused docs and turns them into a concrete code-reading plan before edits or deep analysis.
- A maintenance skill that reviews diffs and keeps intent-focused docs aligned after meaningful code changes.

### Why Two Skills Are The Default

The two workflows look similar on the surface, but they pull the agent in different directions.

- The orientation workflow starts from a question, feature area, component, or behavior that needs explanation.
- The maintenance workflow starts from a diff, branch, pull request, or changed file set that needs documentation verification.
- The orientation workflow tries to stay narrow and follow only the smallest useful reading path.
- The maintenance workflow must widen when shared config or shared code changes have real downstream impact.
- The orientation workflow outputs understanding and next code locations.
- The maintenance workflow outputs a documentation decision and, when appropriate, a documentation edit.

Keeping these as separate skills makes routing clearer, prompts shorter, and guardrails easier to preserve.

## Secondary Recommendation

Generate one combined skill only when the destination repository has a strong reason to minimize artifacts.

If a combined skill is required, keep separate internal branches for:

- pre-change orientation
- post-change maintenance
- diff-base selection
- documentation action selection

Do not blur those branches into a single vague procedure.

Combined mode is intentionally less specified in this blueprint. The split templates are the canonical path. If a destination repository truly needs one combined artifact, treat the merge as a local integration task rather than assuming parity with the split path.

## Agent-First Writing Model

Assume the reader can already inspect files, reconcile conflicting evidence, and choose local conventions.

That means the blueprint should emphasize:

- decision boundaries
- required verification steps
- failure conditions
- explicit placeholders
- edge-case handling

Literal angle-bracket markers are temporary scaffolding, not valid final output.

It should avoid turning into a beginner tutorial or a stack-specific playbook.

## Portability Contract

This blueprint should remain repository-agnostic.

It should intentionally avoid:

- repository names used as if they were universal
- product names used as if they were reusable abstractions
- concrete repo-relative source or documentation paths presented as defaults
- framework-specific directories, namespaces, or class families presented as stack-neutral guidance
- project-specific component names embedded in generic examples
- hardcoded assumptions about language, architecture style, or runtime model

It should intentionally use:

- abstract roles such as component, feature area, service, module, or shared code
- placeholder-style concepts for roots, conventions, and search categories
- procedural rules that can survive translation into another stack
- generic prompt shapes that can be localized only after destination-repository inspection

## What Must Stay Stable Across Ports

These are the invariants that should survive the port into another repository.

### Shared Invariants

- Intent docs are summaries of purpose and reasoning surface, not runtime truth.
- Code verification is required before making behavioral claims.
- The skill should reduce search scope rather than expand it by default.
- The skill should distinguish verified behavior, inferred behavior, and stale documentation.
- The result should optimize for fast, correct orientation instead of exhaustive inventories.

### Orientation-Skill Invariants

- Read repository-wide rules before local interpretation.
- Prefer the smallest useful reading path.
- Turn documentation into a concrete code-reading plan.
- Widen into neighbors only when the question actually depends on them.
- If the local docs are missing or stale, say so explicitly and shift to a code-led explanation without pretending the docs were sufficient.

### Maintenance-Skill Invariants

- Determine diff scope before making documentation freshness claims.
- Choose the smallest correct documentation action for each affected component.
- Do not rewrite docs only because code changed.
- Refresh verification dates only when a real review happened.
- Trace shared-code changes into real consuming components before updating their docs.

## What May Change In The Destination Repository

The destination repository is allowed to adapt:

- skill names
- heading names
- file locations
- placeholder names
- prompt examples
- the exact doc-template headings
- the exact metadata format used to record the blueprint source

The destination repository should not weaken:

- the code-verification requirement
- the difference between pre-change orientation and post-change maintenance
- the rule that docs stay intent-focused
- the rule that shared-code impact must be demonstrated before docs are updated

## Recommended Template Shape

The templates in this repository use a slightly more agent-directed shape than the original seed document.

Recommended sections:

1. YAML front matter with `name` and `description`, the two required Agent Skills fields
2. Title
3. Porting Metadata
4. Goal
5. Invoke When
6. Avoid When
7. Load First
8. Decision Model
9. Procedure
10. Output Contract
11. Validation
12. Prompt Shapes

Why this structure is better for a coding agent:

- `Porting Metadata` makes source traceability explicit.
- `Load First` tells the agent which repository-wide context has priority.
- `Decision Model` captures routing logic that would otherwise be spread across the procedure.
- `Output Contract` tells the agent what shape its answer or edit decision must take.
- `Validation` makes the success conditions testable.

If the destination skill runtime prefers different headings, translate the structure instead of copying the names literally.

The section list above describes the information a generated skill should carry, not a rigid syntax contract. If the destination runtime rejects YAML front matter or custom headings, adapt the schema while preserving the same decision boundaries.

### Front Matter Portability

The front matter is the only part of a generated skill that has a format contract, so it is treated as portable scaffolding rather than as a local convention.

- The Agent Skills specification allows six fields: `name`, `description`, `license`, `compatibility`, `metadata`, and `allowed-tools`. Only `name` and `description` are required.
- Everything else is a runtime extension, such as an autocomplete argument hint, file-scoping patterns, invocation switches, or subagent execution settings.
- Extensions do not travel. A validator for the specification rejects any unknown field and some distribution paths fail on it, while the runtime that defines the field treats it as optional.
- For that reason the templates carry `name` and `description` only. A port may add an extension when the destination runtime documents it, and records that addition in the port note.

Earlier versions of this blueprint put an `argument-hint` field in both templates. That field belongs to a single runtime and not to the specification, so the reference validator rejected the example skills that carried it.

## Search Pattern Guidance

Bindings for entrypoint, registration, config-access, and shared-dependency search patterns are seed cues captured during porting.

They help the generated skill start in the right places, but they are not permanent truth.

When the local runtime allows live repository search at invocation time, prefer current conventions and rediscovery over hardcoding brittle regex that will age quickly.

Hardcode only the smallest stable pattern set that materially improves first-pass navigation.

## Naming Guidance

Prefer names that describe the job.

Good directions:

- `docs-orientation`
- `documentation-orientation`
- `docs-maintenance`
- `documentation-refresh`

Avoid names that overfit this repository.

Examples to avoid unless they already fit the destination repository:

- `portable-read-skill`
- `repobrain-docs-skill`
- `seed-update-skill`

A skill name is also a directory name. The specification requires lowercase letters, digits, and hyphens, at most 64 characters, and a match with the directory that contains the `SKILL.md`.

Before settling on a name, check the destination runtime for reserved or special-cased names and for built-in commands or bundled skills that a project skill with the same name would replace.

## Source-Version Traceability

Every generated skill should carry a record of which version of this blueprint was used.

Minimum data to keep somewhere discoverable:

- blueprint name
- blueprint version
- blueprint source ref or commit, when available
- port date

If the skill format cannot safely include comments or metadata blocks, store the same information in a repository note that later agents can find.

## General Prompt Shapes

Keep prompt examples generic in this blueprint.

Useful shapes:

- orient me in this area before I edit code
- explain how this documented component works using docs plus current code
- check whether the docs for this area drifted from implementation
- refresh the docs affected by this branch
- trace component impact from this shared dependency change

Do not freeze source-repository nouns into the blueprint.

## Blueprint Self-Check

Before changing this blueprint, confirm all of the following:

- it contains no repository-specific path that is being treated as portable default guidance
- it contains no product-specific noun that another repository could not map cleanly
- it contains no stack-specific namespace, controller family, framework directory, or class pattern presented as universal
- code-search examples remain abstract or are clearly labeled as destination-local examples
- conventions are described as inputs the destination repository must provide, not facts this blueprint assumes
- it contains no runtime-specific front matter field presented as portable default guidance

Manual verification procedure:

1. Search for proper nouns that belong only to one source repository.
2. Search for concrete directory trees that were copied from a source repository.
3. Search for stack-specific classes, namespaces, functions, or command patterns that only make sense in one framework.
4. Confirm that path-like references are either generic categories or intentionally destination-local examples.
5. Confirm that role-based terms can still map onto another repository without preserving the original stack.
