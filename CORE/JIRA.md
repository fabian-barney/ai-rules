# JIRA

Guidance for AI agents creating and updating Jira tickets and Jira summaries.

## Scope
- Define Jira-specific ticket-authoring, update, and summary-output rules.
- Apply this file when the issue tracker is Jira (including Jira-integrated
  workflows in GitHub/GitLab delivery pipelines).

## Semantic Dependencies
- Inherit workflow and delivery constraints from
  `CORE/VERSION_CONTROL_SYSTEM.md`.
- Inherit validation priorities from `TEST/TEST.md`,
  `SECURITY/SECURITY.md`, and `COMPLIANCE/COMPLIANCE.md`.
- This file specializes issue-tracker behavior for Jira and does not replace
  branch/PR/MR workflow requirements from `CORE/VERSION_CONTROL_SYSTEM.md`.

## General Jira Guidance
- Keep ticket intent explicit: problem, scope, acceptance criteria, and
  validation focus.
- Keep descriptions structured and scannable; avoid large prose blocks.
- Keep acceptance criteria testable and observable.
- Keep language factual and implementation-neutral where possible.
- Always link related code-delivery artifacts (branch, PR/MR, release notes)
  when reporting status.

## Ticket Description Edit Policy (Mandatory)
- Editing descriptions of existing Jira tickets is forbidden unless explicitly
  requested by the user, Product Owner, or another authorized requester.
- Without explicit request, do not rewrite or "improve" existing ticket
  descriptions.
- Without explicit request, add clarifications/status in comments only.
- If a description appears wrong or incomplete, propose changes in a comment
  and wait for explicit approval before editing the description.
- When explicit edit approval exists, keep changes minimal, preserve original
  intent, and record what changed in a comment.

## Jira Ticket Description Templates
Use these templates when creating new Jira tickets or when explicitly asked to
rewrite a description.

### 1. Story / Feature Template
```md
## Goal

## Problem Statement

## Scope
- In:
- Out:

## Acceptance Criteria
- [ ] AC1
- [ ] AC2

## Dependencies

## Risks

## PO Notes

## QA Notes
```

### 2. Bug Template
```md
## Problem Statement

## Impact
- Users affected:
- Severity:

## Reproduction Steps
1.
2.
3.

## Expected Result

## Actual Result

## Scope
- In:
- Out:

## Acceptance Criteria
- [ ] AC1
- [ ] AC2

## PO Notes

## QA Notes
```

### 3. Technical Debt / Refactor Template
```md
## Motivation

## Current Pain

## Scope
- In:
- Out:

## Success Criteria
- [ ] SC1
- [ ] SC2

## Risks and Mitigations

## Validation Plan

## PO Notes

## QA Notes
```

### 4. Spike / Research Template
```md
## Question to Answer

## Context

## Scope
- In:
- Out:

## Deliverables
- [ ] Decision summary
- [ ] Options considered
- [ ] Recommended next step

## Timebox

## PO Notes

## QA Notes
```

## Jira Summary Templates (Mandatory Fields)
- Keep implementation bullets short and non-detailed.
- Include all MRs/PRs in every delivery summary; do not omit partial or
  supporting merge requests.
- Always include PO and QA guidance on what to validate.

### 1. Single-Ticket Delivery Summary
```md
## Delivery Summary
- Ticket: <JIRA-KEY>
- Status: <Done/In Review/Blocked>
- MRs/PRs:
  - <MR/PR-1 URL>
  - <MR/PR-2 URL>
- Implemented:
  - Short sentence 1.
  - Short sentence 2.
- Acceptance Criteria Status:
  - AC1: Done/Partial/Blocked
  - AC2: Done/Partial/Blocked
- PO Test Focus:
  - Business outcome to verify.
  - User-flow focus area.
- QA Test Focus:
  - Functional scenarios to test.
  - Negative/edge scenarios to test.
- Open Risks / Follow-ups:
  - Risk or follow-up item.
```

### 2. Multi-MR Consolidated Summary
```md
## Consolidated Implementation Summary
- Ticket: <JIRA-KEY>
- Included MRs/PRs:
  - <MR/PR-1 URL> - one short purpose sentence.
  - <MR/PR-2 URL> - one short purpose sentence.
  - <MR/PR-3 URL> - one short purpose sentence.
- Implemented (short bullets):
  - Short sentence 1.
  - Short sentence 2.
  - Short sentence 3.
- PO Validation Guide:
  - Business behavior expected after rollout.
  - Regression-sensitive area for acceptance.
- QA Validation Guide:
  - Core path checks.
  - Error-path and permission checks.
  - Data integrity checks.
- Residual Risks:
  - Remaining known risk.
```

## High-Risk Pitfalls
1. Editing an existing ticket description without explicit request.
2. Posting detailed engineering internals instead of short summary bullets.
3. Omitting one or more related MRs/PRs from delivery summaries.
4. Missing PO/QA validation guidance.
5. Marking acceptance criteria complete without evidence.
6. Mixing assumptions and confirmed facts in status updates.

## Do / Don't Examples
### 1. Description Edits
```text
Don't: rewrite Jira description proactively "to make it cleaner".
Do:    keep description unchanged and post clarifications in a comment.
```

### 2. Summary Granularity
```text
Don't: provide long implementation internals in Jira status summary.
Do:    provide short bullets and link MRs/PRs for technical depth.
```

### 3. QA/PO Guidance
```text
Don't: "Ready for test" with no test focus.
Do:    include explicit PO outcomes and QA scenarios to validate.
```

## Code Review Checklist for Jira Updates
- Was existing description editing avoided unless explicitly requested?
- Are created/updated descriptions structured and testable?
- Are all related MRs/PRs listed in the summary?
- Are implementation bullets short and non-detailed?
- Are PO and QA validation hints explicit and actionable?
- Are acceptance criteria statuses traceable to delivered changes?

## Testing Guidance
- Validate every MR/PR link in Jira summaries.
- Validate acceptance criteria status against delivered artifacts.
- Validate PO/QA guidance covers primary and regression-sensitive flows.
- Validate ticket comments distinguish facts, assumptions, and blockers.

## Override Notes
- Project-specific Jira workflows may add mandatory custom fields, but the
  explicit-ticket-edit authorization rule and summary completeness rules here
  remain mandatory.
