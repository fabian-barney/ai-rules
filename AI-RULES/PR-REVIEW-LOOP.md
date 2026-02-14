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
  - `has_open_review_threads`
  - `has_new_valid_findings`
  - `has_required_checks_green`
  - `merge_gate_passed`

## Loop (Round-Robin, Multi-Issue)
1. Build a queue of active work items.
2. For each item in round-robin order:
   - If code/docs updates are pending, push them.
   - After each push, set `last_push_at` and reset post-push review state:
     `last_review_submitted_at = null`,
     `has_review_in_progress_after_last_push = false`,
     `has_open_review_threads = false`,
     `has_new_valid_findings = false`,
     `has_required_checks_green = false`,
     `merge_gate_passed = false`.
   - Collect PR timeline events, Copilot review status, and review threads.
     - Set `has_review_in_progress_after_last_push` from timeline/status for
       the latest push.
     - Set `last_review_submitted_at` to the latest Copilot review
       `submitted_at` that is after `last_push_at`; otherwise keep it `null`.
     - Set `has_open_review_threads` from unresolved review threads.
     - Set `has_required_checks_green` from required status checks for the
       current PR head commit.
     - Set `merge_gate_passed=true` only when all hard merge gate conditions
       are true; otherwise set `merge_gate_passed=false`.
   - If timeline events show a review currently running for the latest push,
     skip this item for now and continue with the next item (no idle waiting).
   - If `has_required_checks_green = false`, keep the item active, skip it for
     now, and continue with the next item (no idle waiting).
   - If `last_review_submitted_at = null` (no Copilot review submission after
     `last_push_at`), trigger GitHub Copilot Code Review via API and
     continue with the next item:
     - Get raw PR node ID:
       `gh pr view <PR_NUMBER> --json id --jq .id`
     - Request review from Copilot bot:
       `gh api graphql -f query="<MUTATION_QUERY>" -f pr="<PR_ID>" -f bots='copilot-pull-request-reviewer'`
     - Where `<MUTATION_QUERY>` is the complete
       `requestReviewsByLogin` GraphQL mutation and `<PR_ID>` is the value
       returned by the previous command.
     - Verify a new review request/review appears before judging latest state.
   - Classify each Copilot finding as valid or invalid.
   - Set `has_new_valid_findings` from the current round's classification
     result.
   - Reply to each thread with the classification and concise rationale.
   - Fix valid findings, then resolve handled threads.
   - If any changes were pushed, set `last_push_at`, reset the same post-push
     fields listed above, re-trigger GitHub Copilot Code Review via API (same
     steps above), and move on to the next item.
   - If no changes were required or pushed (for example, all findings were
     invalid or already handled), do not push or re-trigger review; move on to
     the next item.
3. Repeat until every active item satisfies all conditions:
   - `last_review_submitted_at != null` (a Copilot review was submitted after
     `last_push_at`)
   - no review is currently running for the latest push
   - no new valid findings remain
   - no open review threads remain

## Hard Merge Gate (No Exceptions)
Before merging any PR in this loop, evaluate this gate explicitly. If any item
is false or unknown, do not merge.
- `last_review_submitted_at > last_push_at`
- `has_review_in_progress_after_last_push = false`
- `has_open_review_threads = false`
- `has_new_valid_findings = false`
- `has_required_checks_green = true`

Definition:
- `merge_gate_passed = true` only when all hard-merge-gate checklist items
  above are true in the same evaluation round.

Operational rules:
- Treat missing/ambiguous timestamps or review state as gate failure.
- Treat missing/ambiguous required-check state as gate failure.
- Passing CI alone is never sufficient for merge.
- A prior Copilot review that predates the latest push never satisfies the gate.
- Never merge in a state where Copilot review is still pending/running for the
  latest push.

## Completion
- If `MERGE_AFTER_CLEAN_LOOP=true`, merge each PR only when:
  - `merge_gate_passed = true` from the hard merge gate above
- If `MERGE_AFTER_CLEAN_LOOP=false`, stop at clean loop and report each PR's
  gate-evaluated status to the user, including `merge_gate_passed` and any
  failing hard-gate conditions. Do not label a PR as merge-ready unless the
  hard merge gate passes.

## Guardrails
- Keep PRs focused; do not bundle unrelated repository changes.
- Never delete review comments to make threads disappear.
- For invalid findings, leave a clear rationale before resolving.
- Never trigger Copilot review via PR comments (for example `@copilot review`
  or `/copilot review`).
- Never mention `@copilot` in PR comments.
- Do not use fixed wait timers (for example, "wait 5 minutes after push") as a
  merge/review gate; use PR timeline and review state instead.
