# Porting Checklist

Use this checklist while generating destination-repository skills from this blueprint.

Treat this file as the authoritative step-by-step procedure. The README summarizes the same flow and constraints.

## Phase 1: Inspect Before You Generate

Confirm the destination repository has been inspected for:

- repository-wide instructions
- architecture overview or equivalent system map
- existing intent-focused docs or the absence of them
- an existing documentation template or review convention
- existing skill files, agent instructions, or memory systems
- the local git workflow and diff-base strategy
- the runtimes that must load the skills, and the skill directories each one scans
- the front matter fields each runtime accepts or rejects

If no repository-wide rules source exists, record that absence explicitly and continue with the architecture overview, local docs, and code conventions. Do not invent a new rules file unless maintainers actually want one.

Inspection is complete only when you have identified, or explicitly confirmed absent, at least the following:

- repository-wide rules source
- architecture overview or system map
- intent-doc root or missing-doc state
- current documentation state and first-pass plan: reuse, normalize, or bootstrap
- source root, shared-code root, and config root when relevant
- existing skills, memory files, or instruction files that may overlap
- maintenance evidence source
- destination skill runtime or schema constraints
- skill location and front matter rules for each runtime that loads the skills

Record this inspection result in a repository note, porting note, or equivalent artifact before generating skills.

Do not generate skills before this inspection is complete.

## Phase 1A: Establish The Documentation Baseline First

Do not port the full skill pair until the destination repository has a usable documentation baseline.

After inspection, choose the lightest viable first-pass path:

- reuse: existing docs already form a stable target for the skills
- normalize: existing docs are useful but need a first pass into a clearer or more consistent shape
- bootstrap: no usable documentation baseline exists yet, so create the smallest one from scratch

If the choice is not obvious, use [decision-guides.md](decision-guides.md). It provides a compact matrix for choosing between reuse, normalize, and bootstrap.

Minimum bootstrap target:

- one lightweight architecture overview or system map
- one stable intent-doc root or local equivalent
- one canonical component or service doc template, or one canonical normalization target for existing docs
- one or two seed docs, or one or two normalized existing docs, for the areas most likely to be queried first

If you need a starting point, adapt [templates/intent-doc-template-sketch.md](../templates/intent-doc-template-sketch.md) into the destination repository's own conventions.

Recommended rollout:

- inspect the current documentation state
- choose reuse, normalize, or bootstrap
- create or normalize the smallest viable documentation baseline
- port the orientation skill first
- validate that the orientation skill can use the new docs meaningfully
- port the maintenance skill only after the documentation target is stable enough for repeatable review

If the destination repository explicitly does not want intent-focused docs, stop and do not port this blueprint.

## Phase 2: Choose The Skill Layout

Default choice: two separate skills.

Choose the default when:

- the repository has enough structure to support distinct pre-change and post-change workflows
- different prompts are likely to trigger different skill behavior
- the repository benefits from tighter routing and smaller procedures

Choose a single combined skill only when:

- the local skill runtime strongly penalizes multiple files
- the repository has very small documentation scope
- the maintainers explicitly prefer one artifact with separate internal branches

Record the decision and the reason.

If you choose a combined skill, treat it as an advanced local adaptation. This blueprint intentionally provides split templates only.

## Phase 3: Bind Local Placeholders

Bind each abstract placeholder to a local equivalent or explicitly mark it unavailable.

Minimum set to resolve:

- repository-wide rules source
- architecture overview source
- intent-doc root
- source root
- config root, if relevant
- shared-code root, if relevant
- canonical doc template or local documentation shape
- entrypoint search pattern
- registration search pattern
- config-access search pattern
- shared-dependency search pattern
- diff-base fallback strategy
- skill directory location
- skill directory name for each generated skill, identical to its `name` field

If a placeholder has no clean local equivalent, document the adaptation instead of silently guessing.

## Phase 3A: Choose The Maintenance Evidence Source

Choose how the generated maintenance skill will establish change scope.

Preferred order:

- explicit user-supplied diff range, pull request target, or review target
- repository-supported git diff or merge-base strategy
- explicit changed-file list or changed-component list
- patch excerpt, review artifact, or CI-produced diff text
- manually curated component list as a last resort

When the later maintenance review reaches the documentation-action decision, use the smallest-correct-action matrix in [decision-guides.md](decision-guides.md) to keep `content update`, `verification-only refresh`, `no change`, and `new doc` decisions consistent.

Require the generated maintenance skill to state which evidence source it used and what blind spots remain when evidence is partial.

## Phase 4: Stop-And-Decide Conditions

Pause and resolve the integration strategy if any of the following are true:

- there is no architecture overview and system boundaries are unclear
- there is no intent-doc system yet
- the repository already has overlapping memory or instruction files
- the source root and shared-code root overlap in a way that breaks clean mapping
- the git workflow makes upstream-based diffing unreliable
- the repository already has similar skills that may conflict with these generated ones
- dynamic registration or indirect dependency wiring makes search-based tracing unreliable
- more than one runtime must load the skills, and they scan no common skill directory

Do not paper over these conditions by hardcoding assumptions into the generated skills.

## Phase 5: Adapt The Templates

While adapting the templates, confirm that:

- local names follow destination-repository conventions
- the generated skills point to local docs, not blueprint placeholders
- every literal `<...>` placeholder marker has been replaced or intentionally removed
- the front matter holds only Agent Skills fields, plus any extension the destination runtime documents
- each skill directory name equals its `name` field
- repository-specific prompt examples are only added after inspection
- local search patterns are treated as seed bindings rather than brittle forever-rules
- the orientation skill still stays narrow by default
- the maintenance skill still chooses the smallest correct documentation action
- the generated skills include source-version traceability

## Phase 6: Validate In The Destination Repository

Run a format check first. Use the destination runtime's validator, or a spec validator if it has none.

Run at least one real prompt for each generated skill.

If the current environment cannot execute a real prompt loop, record that limitation explicitly and do not claim full validation.

Orientation skill validation:

- it loads repository-wide rules before local interpretation
- it identifies the right local docs
- it reconciles docs with code instead of trusting docs blindly
- it produces concrete next code locations
- a near-miss prompt that belongs to the maintenance skill does not trigger it

Maintenance skill validation:

- it chooses the correct diff scope
- it maps changed code to the right documented areas
- it distinguishes content update, verification-only refresh, no change, and new doc
- it avoids rewriting docs when the reasoning surface did not change
- a near-miss prompt that belongs to the orientation skill does not trigger it

Validation should fail if any of the following remain true:

- any literal `<...>` placeholder marker survives in the generated artifacts
- the local bindings cannot be reconstructed from the generated result or repository note
- the maintenance skill does not state the evidence source it used and any blind spots when evidence is partial
- the orientation skill cannot point to concrete next code locations
- the generated skill format is incompatible with the destination runtime
- the format check fails, for example because `name` differs from the skill directory
- a skill triggers on a near-miss prompt

## Phase 7: Record Port Provenance

Before finishing, record:

- blueprint version
- blueprint source ref or commit, when available
- port date
- location of the generated skills
- any local deviations from the default blueprint structure

Store this information where later agents can find it.

## Practical Replication Notes

Keep these three layers separate while porting:

- reusable skill logic: the decision boundaries, verification discipline, and procedure that should survive the port
- local bindings: the destination repository's actual documentation roots, source roots, rules sources, and diff-base strategy
- local search patterns: the code-search examples that reflect the destination repository's language, framework, and wiring model

That separation keeps the result portable and maintainable instead of merely copied.

## Final Validation Checklist

Confirm all of the following:

- an inspection record exists for the destination repository and covers the minimum required inputs
- the documentation baseline decision was recorded as reuse, normalize, or bootstrap
- if no repository-wide rules source existed, that absence was recorded explicitly
- the generated skills fit the destination repository's naming and file layout
- the generated skills do not conflict with existing memory or instruction systems
- no literal `<...>` placeholder remains in the generated artifacts
- the placeholder bindings are discoverable somewhere in the destination repository
- the diff-base strategy matches the local git workflow
- the non-git fallback evidence source is defined when git history is unavailable or unreliable
- the example prompts are generic enough to route well or intentionally localized after inspection
- the generated skill format matches the destination runtime or intentionally omits incompatible template structure
- each generated skill passes the format check, and its directory name equals its `name` field
- every runtime that loads the skills is documented to scan the directory that holds them
- the generated skills clearly distinguish documented intent from verified current behavior
- if combined mode was chosen, the final skill still preserves explicit internal branches for orientation and maintenance
- the generated skills retain a reference to the blueprint version used for the port
