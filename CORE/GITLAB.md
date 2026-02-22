# GITLAB

Guidance for AI agents using GitLab for branch protection, merge requests, and
review lifecycle rules.

## Scope
- Define GitLab-specific delivery and review workflow constraints.
- Apply this file when the code hosting and review platform is GitLab.
- This file does not define GitLab CI pipeline authoring rules.

## Semantic Dependencies
- Inherit baseline branch/PR/MR workflow rules from
  `CORE/VERSION_CONTROL_SYSTEM.md`.
- Inherit review quality expectations from `REVIEW/CODE_REVIEW.md`.
- Inherit security/testing/compliance gates from `SECURITY/SECURITY.md`,
  `TEST/TEST.md`, and `COMPLIANCE/COMPLIANCE.md`.
- This file specializes GitLab platform behavior and does not replace baseline
  VCS workflow requirements.

## Protected Branch Policy (Mandatory)
- Treat protected branches as read-only for AI agents.
- Do not push directly to protected branches.
- Use dedicated feature branches for implementation work.
- Create an MR for any change targeting a protected branch.

## Merge Authority and Merge Gates (Mandatory)
- MR creators must not merge their own MRs.
- The self-merge restriction may be bypassed only when all of the following are
  true:
  - The user gives explicit merge instruction for the specific MR.
  - The user explicitly confirms in the current session that they are a GitLab
    repository owner for the target project and that they authorize bypassing
    the self-merge restriction.
  - You, as an AI agent, treat the user as not a repository owner unless this
    explicit owner confirmation and authorization is present.
- If the above conditions are not met, do not attempt to merge an MR that you
  created or substantially authored.
- Respect MR gates; do not bypass required checks, approvals, or policies.
- Do not use force-merge behavior to skip required gates.
- Merging is forbidden by default without explicit user instruction.
- Never merge an MR while any discussions
  (threads/conversations) are unresolved.
- If asked to merge with unresolved discussions/threads, stop and ask the user
  how to proceed because merge is not allowed in that state.

## Review Conversation Ownership
- Only the author of a review comment may resolve that
  discussion/thread (conversation).
- Do not resolve discussions/threads (conversations) created by other
  reviewers.
- If you authored the comment and the issue is fixed, resolve that
  discussion/thread (conversation).
- Keep discussion history intact; do not delete comments to hide unresolved
  work.

## MR Description Requirements
- When opening an MR, include a short developer-focused implementation summary.
- Include files reviewers may skip because they are generated, copied, or
  standard imports.
- Highlight non-obvious logic and explain why it is implemented that way.
- Keep scope, validation, and residual risk explicit.
- Reuse the general PR/MR summary template from
  `CORE/VERSION_CONTROL_SYSTEM.md`.

## Code Review Workflow for GitLab MRs
- Apply `REVIEW/CODE_REVIEW.md` severity-first process for every MR review.
- Place comments on precise affected lines, not only on the MR overview.
- Use GitLab code suggestions for small, localized fixes.
- If a requested fix is broader than a local suggestion, offer to implement it
  in an AI-agent session on the existing branch.
- After offering broader implementation help, wait for user confirmation before
  making branch changes.

## High-Risk Pitfalls
1. Direct pushes to protected branches.
2. Self-merging MRs without the explicit owner-authorized exception.
3. Resolving another reviewer's conversation.
4. Merging while review discussions/threads (conversations) are unresolved.
5. Bypassing required MR gates or force-merging.
6. MR descriptions that hide generated files or non-obvious logic.

## Do / Don't Examples
### 1. Protected Branches
```text
Don't: push fix commits directly to a protected main branch.
Do:    create a feature branch and open an MR into the protected branch.
```

### 2. Merge Discipline
```text
Don't: merge your own MR because pipeline is green.
Do:    wait for explicit merge instruction and enforce all MR gates.
```

### 3. Conversation Ownership
```text
Don't: resolve a reviewer conversation you did not author.
Do:    reply with the fix, then let the comment author resolve it.
```

## Code Review Checklist for GitLab Workflow
- Is work done on a dedicated feature branch with MR into protected target?
- Were protected-branch and merge-gate rules enforced without bypass?
- Is merge authority valid for the acting user and instruction context?
- Are unresolved review discussions/threads (conversations) blocking merge?
- Are conversation resolutions owned by comment authors?
- Does the MR description include summary, skip candidates, and non-obvious
  rationale?

## Testing Guidance
- Verify branch protection and MR gate configuration before merge actions.
- Verify review conversations are fully resolved by authorized authors.
- Verify MR descriptions include reviewer-focused summary and risk notes.
- Verify no force-merge or gate-bypass actions were used.

## Override Notes
- Project-specific GitLab governance may be stricter, but protected-branch
  discipline, unresolved review discussions/threads (conversations) blocking
  merge, and merge-by-explicit-request behavior remain mandatory.
