---
name: <LOCAL_MAINTENANCE_SKILL_NAME>
description: "Use repository diffs or other change evidence plus local intent docs to decide whether documented components need a content update, verification refresh, new doc, or no change."
argument-hint: "Describe the branch, diff range, pull request, component set, or documentation area to verify"
---

# Documentation Maintenance Skill

Before finalizing a generated skill from this template, replace every literal `<...>` placeholder and adapt or remove any metadata or headings the destination runtime does not support.

## Porting Metadata

Record the blueprint source in a format the destination repository can keep stable.

Suggested marker:

```text
Ported from RepoBrain Skill Blueprint
Blueprint version: 2026.05.06
Blueprint source ref: <tag-or-commit>
Ported on: <YYYY-MM-DD>
```

If comments or freeform metadata conflict with the destination skill runtime, store this information in a repository note instead.

## Goal

Keep the destination repository's intent-focused docs aligned with meaningful code changes without turning them into implementation inventories.

This skill is for post-change verification and documentation maintenance. It is not for first-pass orientation before editing code.

## Invoke When

- A branch, pull request, or commit range changed documented behavior.
- You are working from a changed-file list, patch excerpt, review artifact, or manually supplied change scope instead of a live branch.
- You need to verify whether current docs still match the code.
- You need to decide whether a component doc needs a content edit, verification-only refresh, new doc, or no change.
- Shared configuration or shared code changed and downstream component impact must be checked.

## Avoid When

- The task is to understand a feature area before making edits.
- The repository has no stable doc template and no documented component model yet.
- The request is for exhaustive implementation notes rather than intent-focused docs.

## Load First

Read the destination repository's equivalents for:

- repository-wide instructions or agent rules
- documentation conventions and doc template
- architecture overview or system map
- git workflow assumptions that affect diff-base selection
- the available change evidence source: git history, changed-file list, patch excerpt, review artifact, or manually supplied component list
- any existing memory or instruction files that already track documentation ownership or verification state

If no repository-wide rules source exists, say so explicitly and continue with the architecture overview, docs, and local conventions. Do not invent a new rules file during the port unless maintainers asked for one.

Do not edit docs until diff scope and local conventions are clear.

## Decision Model

For each affected documented area, choose the smallest correct action:

- content update
- verification-only refresh
- no change
- new doc

Never refresh a verification date unless you actually reviewed that area.

When shared config or shared code changes, prove real consumer impact before updating downstream docs.

If git history is unavailable, derive scope from the best available change evidence and state the limitation explicitly.

## Procedure

### 1. Establish Change Evidence And Diff Scope

Pick the comparison range before reasoning about documentation freshness.

Preferred order:

- use an explicit user-supplied diff range, pull request target, or review target when available
- otherwise use the current branch's upstream diff base when that is reliable in the destination repository
- otherwise use the destination repository's documented fallback branch or merge-base strategy
- otherwise use an explicit changed-file list or changed-component list
- otherwise use a patch excerpt, review artifact, or CI-produced diff text

Do not hardcode a branch name unless the destination repository's workflow explicitly requires it.

If the available evidence is incomplete, say so explicitly and carry that uncertainty through the rest of the review. Do not pretend missing history or missing files are irrelevant.

### 2. Build The Candidate Area List

Start from the files, components, or runtime-significant areas identified by the chosen change evidence.

Map those changes to the documented components, services, modules, or feature areas that could be affected.

If the change lands in shared config or shared code, expand the candidate list to the real consumers rather than stopping at the shared file itself.

### 3. Read The Existing Docs Before Deciding

For each candidate doc:

- read the purpose and high-signal guidance
- note the current verification state or review date if the repository tracks one
- preserve concise summaries that are still correct

Do not rewrite a strong summary just because the implementation evolved internally.

### 4. Review The Relevant Code Changes

Verify whether the diff changes any of the following:

- user-visible or operator-visible behavior
- documented purpose or ownership
- priority rules, fallback order, or state transitions
- integration boundaries
- cache behavior, invalidation rules, or performance-sensitive behavior that changes reasoning
- non-obvious gotchas another agent would likely miss

### 5. Choose The Smallest Correct Action

Choose exactly one action per documented area.

#### Content Update

Use this when the diff changed the reasoning surface the doc is supposed to teach.

#### Verification-Only Refresh

Use this when the area was genuinely reviewed and the current doc still teaches the right thing.

#### No Change

Use this when the diff is cosmetic, mechanical, logging-only, test-only, or otherwise irrelevant to the doc's reasoning value.

#### New Doc

Use this when a meaningful documented area exists in code but still lacks an intent-focused doc.

### 6. Write Only High-Signal Updates

Keep docs concise.

- preserve purpose-level summaries
- update rules, gotchas, and cross-component flow only when they materially changed
- avoid inventories of files, hooks, classes, or helper functions unless the destination repository explicitly wants that level of detail

### 7. Handle Shared-Code Changes Carefully

If the change started in shared config or shared code:

- trace actual usage into affected documented areas
- verify each affected doc before touching it
- avoid mass-updating unrelated docs
- update architecture-level docs only if the convention itself changed

If impact cannot be demonstrated from code or repository conventions, do not assume impact.

### 8. Update Discovery Paths When Needed

If a documented area is added, removed, renamed, or moved, update any local documentation index or architecture overview that agents depend on for discovery.

## Output Contract

The result should state:

- the evidence source used
- the diff scope used
- the documented areas reviewed
- the action chosen for each reviewed area
- any documentation drift found
- any blind spots or unresolved impact questions

If edits were made, they should improve usefulness rather than length.

## Validation

Before finishing, confirm that:

- no literal `<...>` placeholder remains in the generated skill
- the skill format still matches the destination runtime
- the diff-base choice matches the destination repository's git workflow
- the evidence source used was stated explicitly, with blind spots when applicable
- if git history was unavailable, the alternative evidence source and its blind spots were stated explicitly
- each reviewed area received the smallest correct action
- verification state changed only where a real review happened
- shared-code changes were traced into real consumers before downstream docs were edited
- the updated docs still read as intent-focused guidance rather than implementation notes

## Prompt Shapes

- `Refresh the intent docs affected by the current branch.`
- `Review this diff and tell me which documented areas need updates.`
- `Given this changed file list, tell me which documented areas need review.`
- `Review this patch excerpt and decide which docs need updates.`
- `Check whether this shared dependency change affects any documented components.`
- `Re-verify the docs for the areas touched by this pull request.`
