# GITHUB

Guidance for AI agents using GitHub for branch protection, pull requests, and
review lifecycle rules.

## Scope
- Define GitHub-specific delivery and review workflow constraints.
- Apply this file when code hosting and review platform is GitHub.
- This file does not define GitHub Actions pipeline authoring rules; see
  `CI-CD/CI-CD.md` for CI/CD pipeline and workflow authoring guidance.

## Semantic Dependencies
- Inherit baseline branch/PR/MR workflow rules from
  `CORE/VERSION_CONTROL_SYSTEM.md`.
- Inherit review quality expectations from `REVIEW/CODE_REVIEW.md`.
- Inherit security/testing/compliance gates from `SECURITY/SECURITY.md`,
  `TEST/TEST.md`, and `COMPLIANCE/COMPLIANCE.md`.
- This file specializes GitHub platform behavior and does not replace baseline
  VCS workflow requirements.

## Protected Branch Policy (Mandatory)
- Treat protected branches as read-only for AI agents.
- Do not push directly to protected branches.
- Use dedicated feature branches for implementation work.
- Create a PR for any change targeting a protected branch.

## Merge Authority and Merge Gates (Mandatory)
- PR creators must not merge their own PRs.
- The self-merge restriction may be bypassed only when all of the following are
  true:
  - The user gives explicit merge instruction for the specific PR.
  - The user explicitly confirms in the current session that they are a GitHub
    repository owner for the target repository and that they authorize
    bypassing the self-merge restriction.
  - You, as an AI agent, treat the user as not a repository owner unless this
    explicit owner confirmation and authorization is present.
- If the above conditions are not met, do not attempt to merge a PR that you
  created or substantially authored.
- Respect branch-protection and ruleset gates; do not bypass required checks,
  required reviews, approvals, or merge policies.
- Do not use admin bypass behavior to skip required gates.
- Merging is forbidden by default without explicit user instruction.
- Never merge a PR while review conversations/threads are unresolved.
- If asked to merge with unresolved conversations/threads, stop and ask the
  user how to proceed because merge is not allowed in that state.

## Review Conversation Ownership
- Prefer the review-comment author to resolve their own
  conversation/thread when possible.
- Do not resolve another reviewer's conversation/thread unless downstream policy
  explicitly allows maintainer resolution or the reviewer has explicitly
  confirmed resolution in the current review context.
- If you authored the comment and the issue is fixed, resolve that
  conversation/thread.
- Keep review history intact; do not delete comments to hide unresolved work.

## PR Description Requirements
- When opening a PR, include a short developer-focused implementation summary.
- Include files reviewers may skim because they are generated, copied, or
  standard imports.
- Highlight non-obvious logic and explain why it is implemented that way.
- Keep scope, validation, and residual risk explicit.
- Reuse the general PR/MR summary template from
  `CORE/VERSION_CONTROL_SYSTEM.md`.

## Code Review Workflow for GitHub PRs
- Apply `REVIEW/CODE_REVIEW.md` severity-first process for every PR review.
- Place comments on precise affected lines, not only on the PR overview.
- Use GitHub code suggestions for small, localized fixes.
- If a requested fix is broader than a local suggestion, offer to implement it
  in an AI-agent session on the existing branch.
- After offering broader implementation help, wait for user confirmation before
  making branch changes.

## High-Risk Pitfalls
1. Direct pushes to protected branches.
2. Self-merging PRs without the explicit owner-authorized exception.
3. Resolving another reviewer's conversation with no explicit policy allowance.
4. Merging while review conversations/threads are unresolved.
5. Bypassing required checks/reviews using admin override paths.
6. PR descriptions that hide generated files or non-obvious logic.

## Do / Don't Examples
### 1. Protected Branches
```text
Don't: push fix commits directly to a protected main branch.
Do:    create a feature branch and open a PR into the protected branch.
```

### 2. Merge Discipline
```text
Don't: merge your own PR because checks are green.
Do:    wait for explicit merge instruction and enforce all merge gates.
```

### 3. Conversation Ownership
```text
Don't: resolve a reviewer conversation with no policy/approval basis.
Do:    reply with the fix and let an authorized owner resolve the thread.
```

## Code Review Checklist for GitHub Workflow
- Is work done on a dedicated feature branch with PR into protected target?
- Were protected-branch and merge-gate rules enforced without bypass?
- Is merge authority valid for the acting user and instruction context?
- Are unresolved review conversations/threads blocking merge?
- Are conversation resolutions handled by an authorized owner?
- Does the PR description include summary, skip candidates, and non-obvious
  rationale?

## Testing Guidance
- Verify branch protection and ruleset requirements before merge actions.
- Verify required checks and required-review policies are active on target
  branch.
- Verify review conversations/threads are fully resolved before merge.
- Verify PR descriptions include reviewer-focused summary and risk notes.
- Verify no admin-bypass merge path was used.

## Override Notes
- Project-specific GitHub governance may be stricter, but protected-branch
  discipline, unresolved review conversations/threads blocking merge, and
  merge-by-explicit-request behavior remain mandatory.
