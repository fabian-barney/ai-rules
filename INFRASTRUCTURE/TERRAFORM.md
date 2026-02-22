# TERRAFORM

Guidance for AI agents implementing and reviewing Terraform infrastructure
changes.

## Scope
- Define Terraform-specific rules for predictable and safe infrastructure
  delivery.
- Apply this file to Terraform configuration, modules, and workflows.

## Semantic Dependencies
- Inherit IaC baseline from `INFRASTRUCTURE/INFRA_AS_CODE.md`.
- Inherit security and compliance constraints from `SECURITY/SECURITY.md` and
  `COMPLIANCE/COMPLIANCE.md`.
- Inherit CI and workflow constraints from `CI-CD/CI-CD.md` and
  `CORE/VERSION_CONTROL_SYSTEM.md`.

## Defaults
- Pin Terraform version and provider versions with explicit constraints.
- Use remote state backends with locking enabled.
- Keep module boundaries clear and reusable; avoid monolithic root configs.
- Keep variable and output contracts explicit and documented.
- Prefer declarative data sources over imperative external scripts.

## Planning and Apply Rules
- Run `terraform fmt` and `terraform validate` before plan/apply.
- Generate and review plan output for every non-trivial change.
- Use non-interactive, reviewed apply workflows in CI for shared environments.
- Block apply when plan includes unexpected destructive operations.

## State and Workspace Governance
- Store state in secured remote backend, never in VCS.
- Scope state per environment/workload to reduce blast radius.
- Use locks to prevent concurrent apply corruption.
- Use workspaces intentionally; avoid hidden multi-environment coupling.
- Keep state access audited and least-privilege.

## Module and Dependency Discipline
- Keep modules cohesive with clear inputs/outputs.
- Avoid circular module dependencies and hidden provider coupling.
- Keep provider aliases explicit when multi-account/multi-region applies.
- Prefer explicit `depends_on` only when implicit graph is insufficient.

## Secrets and Sensitive Data
- Do not commit secrets in `.tf`, `.tfvars`, or generated plan artifacts.
- Mark sensitive outputs as `sensitive = true`.
- Source secrets from secure stores and runtime injection paths.
- Keep CI logs redacted for sensitive values.

## Drift, Import, and Migration
- Run periodic drift checks (`terraform plan`) for critical environments.
- Use `import` and `moved` blocks deliberately during migration/refactors.
- Keep migration steps documented for state/schema changes.
- Validate replacement impact before forcing resource recreation.

## High-Risk Pitfalls
1. Unpinned providers causing unreviewed behavior changes.
2. Local state or unlocked shared state causing corruption.
3. Blind apply without reviewed plan output.
4. Secrets committed in tfvars or exposed via outputs/logs.
5. Overloaded root modules with poor ownership boundaries.
6. Manual console edits causing persistent drift.
7. Force replacement without rollback planning.

## Do / Don't Examples
### 1. Provider Versioning
```text
Don't: leave provider versions floating across environments.
Do:    pin provider versions and review upgrades explicitly.
```

### 2. State Safety
```text
Don't: keep production state in local files.
Do:    use remote backend with locking and audited access.
```

### 3. Plan Discipline
```text
Don't: run apply directly after edit with no plan review.
Do:    review plan diff and apply through gated workflow.
```

## Code Review Checklist for Terraform
- Are Terraform and provider versions pinned appropriately?
- Is remote state backend and locking configured safely?
- Does the plan show expected change scope and no unexplained destruction?
- Are module boundaries/ownership clear and maintainable?
- Are secrets excluded from config, outputs, and logs?
- Are migration/import/replacement steps explicit and low-risk?

## Testing Guidance
- Run `terraform fmt -check` and `terraform validate`.
- Run plan in CI and publish plan summary for review.
- Test module behavior in isolated non-production environments.
- Test drift detection and reconciliation for critical stacks.
- Test rollback/recovery path for destructive or broad-impact changes.

## Override Notes
- Project-specific Terraform patterns may narrow implementation details, but
  version pinning, plan-review discipline, state safety, and secret hygiene
  remain mandatory.
