# GITHUB_ACTIONS

Guidance for AI agents implementing and reviewing GitHub Actions CI/CD
workflows.

## Scope
- Define GitHub Actions pipeline design and quality-gate constraints.
- Apply this file to workflow files under `.github/workflows/` and reusable
  workflow definitions.

## Semantic Dependencies
- Inherit CI/CD baseline from `CI-CD/CI-CD.md`.
- Inherit build/security/testing constraints from
  `BUILD_TOOLS/**`, `SECURITY/SECURITY.md`, and `TEST/TEST.md`.
- Inherit VCS workflow requirements from `CORE/VERSION_CONTROL_SYSTEM.md`.

## Workflow Defaults
- Keep workflows deterministic and fail fast.
- Separate jobs/stages clearly (lint, build, test, security, package, deploy).
- Keep triggers (`on:`) explicit and minimal by event/branch/path.
- Keep permissions explicit with least privilege at workflow and job level.
- Pin third-party actions by full commit SHA rather than floating tags.

## Quality Gates
- Treat lint, tests, and security scans as merge/release gates.
- Keep required gates non-optional for protected branches.
- Publish test, lint, and coverage reports for review visibility.
- Fail workflows on critical dependency/security findings per policy.

## Release Workflow Rules
- Release workflows are triggered by semantic version tags
  (for example `vMAJOR.MINOR.PATCH`) or explicit release dispatch.
- Release workflows must run full build/test/security checks.
- Release artifacts and reports must be reproducible from tag alone.
- Keep release workflows immutable and auditable.
- Keep rollback and rerun strategy documented.

## Secrets and Security
- Use GitHub Secrets/Variables and environment protection rules for credentials.
- Do not print secrets or sensitive payloads in workflow logs.
- Prefer OIDC-based short-lived cloud credentials over long-lived static keys.
- Restrict deploy jobs to protected branches/tags and required approvals.
- Keep `GITHUB_TOKEN` permissions minimal; elevate only where required.

## Caching and Artifacts
- Use caches for dependency acceleration with safe keys
  (lockfiles/runtime versions).
- Use artifacts for reproducible cross-job handoff, not implicit workspace
  assumptions.
- Keep artifact retention policy explicit and cost-aware.
- Keep matrix strategies bounded to avoid excessive CI spend/noise.

## Observability and Debuggability
- Keep workflow/job logs actionable and concise.
- Emit clear failure context (what failed, where, likely next action).
- Track workflow duration/flakiness trends.
- Keep flaky tests/jobs quarantined and actively remediated.

## High-Risk Pitfalls
1. Floating action versions introducing unreviewed behavior changes.
2. Broad `GITHUB_TOKEN` permissions across all jobs by default.
3. Optional quality gates on protected branches.
4. Secret leakage through logs or committed workflow config.
5. Release workflows bypassing full validation gates.
6. Fragile cache keys causing stale/corrupt build state.
7. Overly broad trigger rules running deploy jobs unexpectedly.

## Do / Don't Examples
### 1. Action Pinning
```text
Don't: uses: actions/checkout@v4
Do:    uses: actions/checkout@<full-commit-sha>
```

### 2. Token Permissions
```text
Don't: permissions: write-all
Do:    set minimal read/write scopes per workflow/job need.
```

### 3. Release Safety
```text
Don't: publish release artifacts without test/security dependencies.
Do:    enforce full gate chain before publish/deploy.
```

## Code Review Checklist for GitHub Actions
- Are workflow triggers explicit and least-surprise?
- Are stages/gates explicit and complete?
- Are merge/release quality gates enforced?
- Are action versions pinned by full SHA?
- Are token permissions and secret handling least-privilege and safe?
- Are cache/artifact strategies deterministic and safe?
- Is release reproducibility from tag alone guaranteed?

## Testing Guidance
- Validate workflow YAML syntax and execution logic before merge.
- Run workflow changes on PR branches and verify required gates.
- Test release workflow on pre-release tags in staging when possible.
- Test failure scenarios (missing secrets, failing tests, vulnerability gates).
- Verify deploy jobs honor environment protections/required approvals.

## Override Notes
- Project-specific delivery policies may add stricter approvals/compliance
  gates, but deterministic quality-gated workflows, least-privilege
  permissions, and secret hygiene remain mandatory.
