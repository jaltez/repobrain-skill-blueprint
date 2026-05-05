---
name: <LOCAL_ORIENTATION_SKILL_NAME>
description: "Use local intent docs plus targeted code verification to orient in a feature area before editing code or making behavioral claims."
argument-hint: "Describe the feature area, component, workflow, or question to understand"
---

# Documentation Orientation Skill

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

Build a correct mental model of a local component, feature area, or workflow by reading the destination repository's intent-focused docs first and then verifying the relevant behavior in code.

This skill is for orientation, explanation, and scoping. It is not for refreshing docs after a diff.

## Invoke When

- You need context before editing code.
- You need to explain the purpose or behavior of a documented area.
- You need to turn local documentation into a concrete code-reading plan.
- You suspect local docs drifted from implementation and need a scoped verification.

## Avoid When

- The real task is to refresh docs after a branch, pull request, or commit range.
- The repository has no usable intent-doc system and the user expects documentation maintenance.
- The request is for an exhaustive file inventory rather than a scoped orientation.

## Load First

Read the destination repository's equivalents for:

- repository-wide instructions or agent rules
- architecture overview or domain map
- the relevant intent-doc root
- any local memory or instruction system that could redirect where docs live

If no repository-wide rules source exists, say so explicitly and continue with the architecture overview, local docs, and code conventions. Do not invent a new rules file during the port unless maintainers asked for one.

Treat local docs as intent summaries, not as runtime truth.

## Decision Model

1. Establish the smallest plausible scope from the user's request.
2. Load repository-wide rules before local component docs.
3. Read only the docs that are directly relevant to the question.
4. Convert the docs into likely code entry points and verification targets.
5. Reconcile doc intent with current code.
6. Widen into adjacent components only if the answer depends on them.

If the prompt is ambiguous, prefer the narrowest reasonable scope. Ask for clarification only when narrowing would materially risk a wrong answer.

If the local docs are missing, clearly stale, or structurally inconsistent, say so and switch to a code-led orientation without pretending the docs were authoritative.

## Procedure

### 1. Establish The Scope

Start from the user's explicit component name, feature area, bug, or workflow question.

If the request uses only domain language, map it to likely local components using the architecture overview and targeted code search.

### 2. Load Shared Context

Read the repository-wide rules that define:

- component or package boundaries
- documentation expectations
- entrypoint conventions
- shared configuration and shared-code conventions

Read the architecture overview when the request spans multiple domains or when naming is ambiguous.

### 3. Read The Relevant Intent Docs

For each relevant local doc:

- capture the purpose in one or two sentences
- extract only the highest-signal rules, caveats, or gotchas
- note cross-component links only when they matter to the user's question

### 4. Convert Docs Into A Verification Plan

Before reading broadly, map the docs to likely code locations.

Bind the destination repository's local search cues explicitly:

- entrypoint search pattern: `<ENTRYPOINT_SEARCH_PATTERN>`
- registration search pattern: `<REGISTRATION_SEARCH_PATTERN>`
- config-access search pattern: `<CONFIG_ACCESS_SEARCH_PATTERN>`
- shared-dependency search pattern: `<SHARED_DEPENDENCY_SEARCH_PATTERN>`

Typical search categories:

- local entry points
- registration or routing sites
- configuration access
- shared service or shared library usage

Use the destination repository's actual search patterns, not the abstract placeholders from the blueprint.

Treat bound search patterns as seed cues. When the local runtime allows live repository search at invocation time, prefer current conventions over stale hardcoded patterns.

### 5. Reconcile Docs With Code

Verify only the behavior that matters to the current question.

Check for:

- drift between documented intent and current code
- ownership boundaries that changed
- priority rules, fallback order, or state transitions that differ in implementation
- dependencies or integrations that the docs describe incorrectly or incompletely

If docs and code conflict, trust code for current behavior and call the drift out explicitly.

### 6. Produce The Orientation

Respond with:

- what the area is responsible for
- the non-obvious rules or caveats that matter
- the main inbound and outbound dependencies that affect the answer
- the concrete next code locations worth reading
- any unverified assumptions or documentation gaps

### 7. Stop Scope Creep

Do not widen into adjacent systems just because they exist. Expand only when the answer truly depends on cross-component flow, shared configuration, or shared code.

## Output Contract

The result should distinguish:

- documented intent
- verified current behavior
- inference or uncertainty

The result should leave the user with specific next code locations rather than a generic summary.

## Validation

Before finishing, confirm that:

- no literal `<...>` placeholder remains in the generated skill
- the local search cues were either bound explicitly or intentionally removed because they had no clean local equivalent
- the skill format still matches the destination runtime
- repository-wide rules were loaded before local interpretation
- the explanation did not treat docs as runtime truth
- any unresolved claim is labeled as inferred or unverified
- the proposed next code locations are concrete enough for follow-up work

## Prompt Shapes

- `Orient me in this area before I edit code.`
- `Explain how this documented component works using the local docs and current implementation.`
- `Check whether the docs for this workflow drifted from the code.`
- `Use the repository docs to tell me where I should read first.`
