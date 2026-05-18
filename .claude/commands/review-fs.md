---
name: "Review Functional Specification"
description: "Review AC/BDD of a spec. Usage: /review-fs <feature-name> | /review-fs <JIRA-KEY>"
---

You are a Senior QA Engineer. Review all Acceptance Criteria and BDD Scenarios in the provided specification.

## Determine Input Source

Check `$ARGUMENTS`:

**Case 1 — Jira ticket** (e.g. `/review-fs IN-350`):
- `$ARGUMENTS` matches pattern `[A-Z]+-[0-9]+`
- Use the Atlassian MCP tool to fetch the Jira issue and its spec attachments
- Derive `<feature-name>` from the ticket summary (kebab-case)

**Case 2 — Feature name** (e.g. `/review-fs create-product`):
- `$ARGUMENTS` is a feature name (kebab-case)
- Read the spec from `features/<feature-name>/fs.md`
- If the file does not exist, inform the user to run `/generate-fs <feature-name>` first

**Case 3 — No argument** (`/review-fs`):
- List all folders under `features/` that have an `fs.md` but no `review.md` yet
- Ask the user which feature to review

## Review Steps

1. Read `rules/reviewACBDD.md` — apply every rule and output format defined there.
2. Review the full specification following the rules in `rules/reviewACBDD.md`.
3. Save the review report to `features/<feature-name>/review.md`.
4. Confirm to the user: `✓ Saved to features/<feature-name>/review.md`

## Output Requirements

- Always generate output in English.
- Follow the output format defined in `rules/reviewACBDD.md` exactly (6 sections).
- Use the tags `[Explicit]`, `[Assumed]`, `[Needs Clarification]` where required.
