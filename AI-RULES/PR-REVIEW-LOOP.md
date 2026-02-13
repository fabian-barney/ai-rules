# PR-REVIEW-LOOP

Repository-standard PR review loop for ai-rules maintenance.

## Scope
- Applies only to this repository.
- Do not copy these rules into downstream-projects.

## Session Merge Preference (Ask Once)
- At the beginning of a session that uses this loop, ask exactly once:
  "After the loop reaches zero valid Copilot findings, should I also merge the
  PR?"
- Store the user's answer as session preference:
  `MERGE_AFTER_CLEAN_LOOP=true|false`.
- Do not ask this merge question again in the same session, even when handling
  multiple issues or PRs.
- After this one-time decision, run unattended through the loop unless blocked.

## Standard Loop
1. Create a focused branch and PR.
2. Push changes.
3. Wait at least 5 minutes after push before collecting Copilot review results.
4. Collect all Copilot review comments and review threads.
5. Classify each Copilot finding as valid or invalid.
6. Reply to each thread with the classification and concise rationale.
7. Fix valid findings in code/docs and push updates.
8. Resolve all handled review threads.
9. If any changes were pushed, re-trigger GitHub Copilot Code Review.
10. Repeat steps 4-9 until no new valid findings remain.

## Completion
- If `MERGE_AFTER_CLEAN_LOOP=true`, merge the PR after the loop is clean.
- If `MERGE_AFTER_CLEAN_LOOP=false`, stop after clean loop and report that the
  PR is ready for user merge.

## Guardrails
- Keep PRs focused; do not bundle unrelated repository changes.
- Never delete review comments to make threads disappear.
- For invalid findings, leave a clear rationale before resolving.
