# INFRA_AS_CODE

Guidance for AI agents implementing and reviewing Infrastructure as Code (IaC)
changes.

## Scope
- Define tool-neutral IaC guardrails for provisioning and evolving
  infrastructure safely.
- Apply this file when infrastructure definitions are changed through code
  (for example, Terraform, Pulumi, CloudFormation, or Ansible-managed resources).

## Semantic Dependencies
- Inherit infrastructure baseline from `INFRASTRUCTURE/INFRASTRUCTURE.md`.
- Inherit security and compliance constraints from `SECURITY/SECURITY.md` and
  `COMPLIANCE/COMPLIANCE.md`.
- Inherit testing and delivery workflow expectations from `TEST/TEST.md`,
  `CI-CD/CI-CD.md`, and `CORE/VERSION_CONTROL_SYSTEM.md`.

## IaC Defaults
- Keep infrastructure declarative, versioned, and reproducible.
- Treat IaC as the source of truth; avoid manual console drift.
- Keep changes small, reviewable, and environment-scoped.
- Prefer immutable replacements over ad-hoc in-place mutation for high-risk
  components where feasible.

## Change Management and Approval Gates
- Require plan/preview output before apply for every non-trivial change.
- Separate plan generation from apply execution in CI/CD where possible.
- Gate production apply with explicit approval controls.
- Block apply when plan/preview shows unexpected destructive impact.

## State, Drift, and Convergence
- Keep state backends protected, access-controlled, and auditable.
- Use locking/concurrency controls to avoid concurrent apply corruption.
- Run periodic drift detection and reconcile intentionally.
- Do not normalize unmanaged/manual changes as acceptable steady state.

## Secrets, Identity, and Access
- Do not commit secrets or static privileged keys in IaC code or variables.
- Prefer short-lived credentials (for example OIDC/workload identity) over
  long-lived static credentials.
- Enforce least privilege for provisioning identities and runtime roles.
- Keep sensitive outputs minimized and redacted in logs/artifacts.

## Environment and Isolation Rules
- Keep dev/test/stage/prod separation explicit in code and state.
- Prevent cross-environment resource references unless explicitly required and
  reviewed.
- Keep naming/tagging/ownership metadata mandatory for auditability and cost
  control.

## Reliability and Rollback
- Define rollback strategy before risky changes (revert, redeploy, or
  replacement path).
- Validate dependency ordering for create/update/delete operations.
- Prefer staged rollout for broad-impact infrastructure changes.
- Keep disaster-recovery-sensitive resources protected from accidental
  destruction.

## High-Risk Pitfalls
1. Applying changes without reviewing plan/preview output.
2. Shared state backend without locking or access boundaries.
3. Manual console changes causing unmanaged drift.
4. Static privileged credentials embedded in IaC config.
5. Cross-environment coupling that allows blast-radius expansion.
6. Destructive changes with no tested rollback path.
7. Weak tagging/ownership metadata blocking operations and cost governance.

## Do / Don't Examples
### 1. Plan/Apply Discipline
```text
Don't: apply infrastructure changes directly from a local machine with no
       reviewed plan output.
Do:    generate plan/preview, review it, and apply through gated workflow.
```

### 2. Drift Handling
```text
Don't: keep manual console edits as undocumented permanent state.
Do:    codify approved changes in IaC and converge environments.
```

### 3. Secret Safety
```text
Don't: store cloud access keys in plain IaC variable files.
Do:    use secret managers and short-lived identity federation.
```

## Code Review Checklist for IaC
- Is plan/preview evidence provided and reviewed?
- Are destructive operations expected, justified, and gated?
- Is state backend protection/locking configured correctly?
- Are identity permissions least-privilege for provisioning and runtime?
- Are environment boundaries explicit and safe?
- Are rollback and failure-handling paths documented?
- Are tags/metadata sufficient for ownership/audit/cost control?

## Testing Guidance
- Validate IaC syntax and policy checks in CI.
- Validate plan/preview output for expected resource diff.
- Test apply in non-production before production rollout.
- Test rollback/recovery path for high-impact changes.
- Test drift detection and reconciliation for critical stacks.

## Override Notes
- Tool-specific IaC docs may specialize implementation details, but plan/apply
  gating, state safety, least privilege, and drift governance remain mandatory.
