# 2026-02-22-no-general-semantics-change-for-scope-limited-rules

## Scope
- Applies only to this repository.
- Do not copy these rules into downstream-projects.

## Issue
While adding `CORE/GITLAB.md`, unrelated baseline wording in
`CORE/RULE_DEPENDENCY_TREE.md` was changed (`Always inherited first`), which
introduced avoidable semantics churn outside the requested GitLab-specific
scope.

## Prevention
- Keep issue scope strict: when adding a specialized rule doc, avoid changing
  baseline semantics in existing parent docs.
- Restrict parent-doc edits to minimal indexing/linking needed to make the new
  specialized rule discoverable.
- If a review suggestion expands into general-semantics changes, defer that
  work to a separate issue/PR.
