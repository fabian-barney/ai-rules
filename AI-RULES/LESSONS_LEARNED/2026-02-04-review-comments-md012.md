# 2026-02-04-review-comments-md012

## Scope
- Applies only to this repository.
- Do not copy these rules into consuming projects.

## Issue
Markdownlint failures (MD012 multiple blank lines) repeated across many files after
bulk refactors, and review comments were deleted instead of resolved.

## Prevention
- After large doc or path changes, run a repo-wide check for multiple blank lines
  and fix MD012 before pushing.
- Resolve PR review comments instead of deleting them.
- Keep files UTF-8 without BOM during bulk edits.
