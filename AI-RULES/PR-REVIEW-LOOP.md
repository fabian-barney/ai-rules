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

## Work Item Model
- Treat each issue/PR pair as one independent work item.
- Track per item:
  - `last_push_at`
  - `last_review_submitted_at`
  - `has_review_in_progress_after_last_push`
  - `has_review_submission_after_last_push`
  - `has_open_review_threads`
  - `has_new_valid_findings`

## Loop (Round-Robin, Multi-Issue)
1. Build a queue of active work items.
2. For each item in round-robin order:
   - If code/docs updates are pending, push them.
   - After each push, set `last_push_at` and reset post-push review state:
     `last_review_submitted_at = null`,
     `has_review_in_progress_after_last_push = false`,
     `has_review_submission_after_last_push = false`,
     `has_open_review_threads = false`,
     `has_new_valid_findings = false`.
   - Collect PR timeline events, Copilot review status, and review threads.
   - If timeline events show a review currently running for the latest push,
     skip this item for now and continue with the next item (no idle waiting).
   - If no Copilot review submission exists after `last_push_at` (based on
     review `submitted_at`), trigger GitHub Copilot Code Review and continue
     with the next item.
   - Classify each Copilot finding as valid or invalid.
   - Reply to each thread with the classification and concise rationale.
   - Fix valid findings, then resolve handled threads.
   - If any changes were pushed, set `last_push_at`, reset the same post-push
     fields listed above, re-trigger GitHub Copilot Code Review, and move on to
     the next item.
3. Repeat until every active item satisfies all conditions:
   - at least one Copilot review submission happened after `last_push_at`
   - no review is currently running for the latest push
   - no new valid findings remain
   - no open review threads remain

## Completion
- If `MERGE_AFTER_CLEAN_LOOP=true`, merge each PR only when:
  - `last_review_submitted_at > last_push_at`
  - no review is in progress
  - no open review threads remain
- If `MERGE_AFTER_CLEAN_LOOP=false`, stop at clean loop and report each PR as
  ready for user merge.

## Guardrails
- Keep PRs focused; do not bundle unrelated repository changes.
- Never delete review comments to make threads disappear.
- For invalid findings, leave a clear rationale before resolving.
- Never trigger Copilot review via PR comments (for example `@copilot review`
  or `/copilot review`).
- Do not use fixed wait timers (for example, "wait 5 minutes after push") as a
  merge/review gate; use PR timeline and review state instead.
