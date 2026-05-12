# Decision Guides

Use this document when a port is blocked by a close call rather than a missing fact.

It turns two judgment-heavy parts of the blueprint into explicit tie-break rules:

- choosing `reuse`, `normalize`, or `bootstrap` for the initial documentation baseline
- choosing `content update`, `verification-only refresh`, `no change`, or `new doc` during maintenance review

## Documentation Baseline Matrix

| Path | Choose It When | Avoid It When | First Move |
| --- | --- | --- | --- |
| `reuse` | Current docs already define stable component boundaries, live in one discoverable place, and are specific enough that another agent could answer a scoped orientation prompt without guessing. | The docs are scattered, mostly stale, or mix intent with low-signal implementation inventory. | Keep the current doc root, record it in the port note, and limit cleanup to the docs that are clearly blocking orientation. |
| `normalize` | Current docs contain useful intent, but the shape, naming, or verification state is inconsistent enough that agents would misread or skip them. | The docs have almost no reusable signal, or there is no agreed doc root to normalize toward. | Pick one canonical doc shape, normalize one or two high-value docs first, and treat that normalized result as the target pattern for the rest. |
| `bootstrap` | There is no usable intent-doc baseline, or the existing material is mostly runbooks, raw inventories, or stale architecture notes that cannot safely guide orientation. | Maintainers do not want an intent-doc system, or an existing doc model already works well enough after light cleanup. | Create the smallest viable set: architecture overview, intent-doc root, canonical template, and one or two seed docs. |

### Baseline Tie-Break Rules

- If a scoped orientation prompt could already be answered from current docs with only targeted code verification, prefer `reuse`.
- If the answer is present but buried under inconsistent headings, naming, or review state, prefer `normalize`.
- If the answer is absent or structurally unreliable, prefer `bootstrap`.

## Smallest Correct Action Matrix

| Action | Choose It When | Do Not Choose It When | Expected Documentation Response |
| --- | --- | --- | --- |
| `no change` | The diff is cosmetic, mechanical, test-only, logging-only, or otherwise leaves the reader-facing reasoning surface unchanged. | You did not review the area and are only assuming the doc is still fine. | Leave the doc untouched. Do not refresh any verification marker. |
| `verification-only refresh` | You reviewed the area end to end and the doc still teaches the right purpose, rules, and boundaries. | The doc is missing a rule, caveat, ownership change, or new cross-component dependency. | Update only review metadata or a brief verification note if the destination repository tracks one. |
| `content update` | The diff changed what another engineer needs to understand: purpose, ownership, rules, fallback order, integrations, or other reasoning-critical behavior. | The code changed internally but the intent-level explanation still holds. | Edit only the sections whose reasoning changed, and preserve concise summaries that are still correct. |
| `new doc` | The diff introduced or exposed a meaningful component or workflow that lacks any intent-focused doc. | An existing doc already covers the area and only needs a scoped update. | Create the smallest viable doc and add it to discovery paths such as the intent-doc index or architecture overview. |

### Maintenance Tie-Break Rules

- If a change starts in shared code or config, default to no downstream doc change until real consumer impact is demonstrated.
- If you are choosing between `no change` and `verification-only refresh`, choose `no change` unless a human or agent actually re-reviewed the area.
- If you are choosing between `verification-only refresh` and `content update`, choose `content update` only when a future reader would need different guidance to reason correctly.
- If you are choosing between `content update` and `new doc`, create a new doc only when the change introduces a distinct documented area rather than expanding an existing one.
