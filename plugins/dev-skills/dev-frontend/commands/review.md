---
description: Review changes since a fixed point along two axes — Standards (repo coding standards) and Spec (originating issue/spec) — run as parallel sub-agents reported side by side
argument-hint: "<commit, branch, or tag>"
---

# /review -- Two-Axis Code Review

Apply the `dev-code-review` skill to review the diff between `HEAD` and: $ARGUMENTS

The fixed point may be a commit, branch, tag, or merge-base. The skill runs two reviews in parallel sub-agents:

- **Standards** — does the code conform to this repo's documented coding standards?
- **Spec** — does the code match what the originating issue/spec asked for?

Reports both axes side by side.
