# Integration Warnings

Use this document before finalizing a destination-repository port.

## Existing Memory Or Instruction Systems

Risk:

The destination repository may already have repository memory, agent instruction files, or skill-specific metadata. Adding a second overlapping system can create conflicting source-of-truth rules about where docs live, how components are named, or which workflow should run.

Verify:

- whether repository-wide instructions already define documentation behavior
- whether memory files already record component mappings or review dates
- whether another skill already covers documentation orientation or maintenance

Preferred response:

- reuse the existing mechanism when it is good enough
- extend it when a small addition is sufficient
- keep separate systems only when the boundary is explicit and discoverable

## Prompt Ambiguity

Risk:

The prompts that will eventually invoke the generated skills are unknown. A broad prompt can accidentally trigger repo-wide exploration or an incorrect component mapping.

Verify:

- whether local terminology is stable
- whether component names differ from user-facing domain language
- whether the skill runtime supports clarification when ambiguity is material

Preferred response:

- scope to the smallest plausible area by default
- add local prompt examples only after inspecting real repository nouns
- ask for clarification only when narrowing would create a high risk of a wrong answer

## Inspection Not Complete

Risk:

Porting can look ready before the minimum repository facts are known. Generating skills before the porter has identified core roots, overlaps, evidence sources, and runtime constraints leads to ports that sound plausible but bind the wrong local system.

Verify:

- whether the repository-wide rules source is identified or confirmed absent
- whether the architecture overview, source roots, and intent-doc root are identified or confirmed absent
- whether overlapping skills, memory files, or instruction files were checked
- whether the maintenance evidence source was chosen
- whether the destination runtime or schema constraints were verified

Preferred response:

- record the missing facts in a port note or repository note
- stop generation until the minimum inspection set is complete
- do not substitute generic template values for unresolved bindings

## Git Workflow Mismatch

Risk:

The maintenance workflow can fail if it assumes `@{upstream}` works everywhere. Detached heads, rebase-heavy workflows, forks, or nonstandard remote tracking can break upstream-based diffing.

Verify:

- how local branches track remotes
- whether maintainers use merge-base, upstream, or explicit branch names for review
- whether CI or automation runs in a detached state

Preferred response:

- define a repository-specific diff-base strategy
- keep a documented fallback branch or merge-base rule
- avoid hardcoding a branch name unless the repository explicitly standardizes it

## No Git History Or Patch-Only Inputs

Risk:

Some environments do not expose usable git history at all. Review tools, CI jobs, or external coding agents may only have a changed-file list, patch excerpt, review artifact, or manually supplied component set.

Verify:

- whether the agent will always run inside a live git checkout
- whether changed-file manifests or patch excerpts are available in automation
- whether maintainers expect the maintenance workflow to operate from review artifacts rather than repository history

Preferred response:

- define an evidence-source order instead of assuming git is always present
- let the maintenance skill accept changed-file lists, patch excerpts, review artifacts, or explicit component lists
- require the maintenance skill to state the evidence source it used and any blind spots caused by missing history

## Missing Intent-Doc System

Risk:

The maintenance skill depends on the existence of intent-focused docs or at least a stable documentation target. Without that, it has nowhere safe to apply its smallest-correct-action model.

Verify:

- whether component or feature docs already exist
- whether maintainers want a canonical doc template before skill generation
- whether the documentation system is stable enough for consistent updates

Preferred response:

- create or agree on the doc model first
- create a lightweight architecture overview, one intent-doc root, and one canonical doc template before full skill rollout
- generate the orientation skill earlier than the maintenance skill if necessary
- delay maintenance-skill rollout until the target documentation shape is real
- if maintainers explicitly do not want intent-focused docs, do not port this blueprint

## Monorepo Or Overlapping Roots

Risk:

Source, config, and shared-code roots may overlap or repeat across packages. A simple placeholder mapping can become ambiguous and create bad search scope.

Verify:

- whether the repository has multiple packages with their own doc systems
- whether shared libraries sit inside the main source tree
- whether one architecture overview is enough or package-specific overviews are required

Preferred response:

- bind placeholders per package or per bounded area when needed
- narrow the generated skills to the intended package scope
- avoid pretending a monorepo has one clean universal root if it does not

## Dynamic Registration Or Indirect Wiring

Risk:

Search-driven tracing can miss real impact when dependencies are wired indirectly through reflection, registries, conventions, or generated code.

Verify:

- whether local dependency discovery is mostly explicit or mostly indirect
- whether there are stable registries, manifests, or generated artifacts that should be searched instead
- whether dynamic behavior creates blind spots for downstream impact analysis

Preferred response:

- document the blind spots in the generated maintenance skill
- add repository-specific fallback checks where explicit usage search is weak
- avoid claiming impact is absent when the repository model makes proof difficult

## Existing Similar Skills

Risk:

The destination repository may already have one or more skills that cover orientation, documentation refresh, or codebase explanation. Generating another pair without reconciliation can fragment routing and confuse future maintainers.

Verify:

- whether current skills already solve part of the same problem
- whether a small edit to an existing skill would be better than adding new ones
- whether naming collisions or prompt collisions are likely

Preferred response:

- merge into existing skills when the fit is clean
- add new skills only when they create a clear new boundary
- document the relationship between old and new skills explicitly

## Skill Runtime And Metadata Format Constraints

Risk:

Some skill runtimes tolerate only a narrow schema, reject YAML front matter entirely, or dislike extra comments and freeform metadata. A source-version marker or an unmodified template header can break loading or routing.

Verify:

- whether the local skill runtime accepts YAML front matter at all
- which fields are required, optional, or forbidden
- whether the local skill format allows comments, extra sections, or custom metadata
- whether provenance should live inside the skill or in a separate repository note

Preferred response:

- adapt or remove front matter to match the local runtime
- keep provenance in the skill when safe
- otherwise store the provenance in a repository note or adjacent index file
- never assume the template schema is portable as written
- never sacrifice runtime compatibility just to keep metadata inline
