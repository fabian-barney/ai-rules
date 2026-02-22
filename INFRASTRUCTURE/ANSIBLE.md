# ANSIBLE

Guidance for AI agents implementing and reviewing Ansible automation changes.

## Scope
- Define Ansible-specific rules for safe, repeatable infrastructure and
  configuration automation.
- Apply this file to Ansible playbooks, roles, inventories, and CI automation.

## Semantic Dependencies
- Inherit IaC baseline from `INFRASTRUCTURE/INFRA_AS_CODE.md`.
- Inherit security and compliance constraints from `SECURITY/SECURITY.md` and
  `COMPLIANCE/COMPLIANCE.md`.
- Inherit CI and workflow constraints from `CI-CD/CI-CD.md` and
  `CORE/VERSION_CONTROL_SYSTEM.md`.

## Defaults
- Keep playbooks idempotent and deterministic.
- Keep role boundaries cohesive and reusable.
- Keep inventory and variable scopes explicit per environment.
- Prefer module-based tasks over shell/command calls where possible.
- Keep privilege escalation explicit, minimal, and auditable.

## Execution and Change Rules
- Run syntax checks/lint before execution in shared branches.
- Use check mode/dry-run for preview where supported.
- Keep high-impact plays gated with explicit approval in CI workflows.
- Keep rollback/remediation steps documented for risky changes.

## Inventory and Variable Governance
- Keep inventory ownership and environment boundaries explicit.
- Avoid global variable sprawl; keep variable precedence predictable.
- Do not embed secrets in plaintext variable files.
- Use vault/secret-manager integrations for sensitive values.

## Role and Task Discipline
- Keep tasks small and focused; avoid monolithic playbooks.
- Keep handlers explicit for service restart/reload side effects.
- Avoid hidden cross-host coupling and ordering assumptions.
- Use tags intentionally for scoped execution paths.

## Security and Access
- Keep SSH/API credentials out of repository content.
- Enforce least privilege for automation accounts.
- Avoid broad `become: true` defaults across all tasks.
- Keep logs free of sensitive values (`no_log` where needed).

## High-Risk Pitfalls
1. Non-idempotent shell scripts masquerading as configuration management.
2. Plaintext secrets in inventory/group vars.
3. Unbounded privilege escalation across playbooks.
4. Environment inventory mixing that expands blast radius.
5. Task ordering assumptions that break under parallelism/partial runs.
6. Missing dry-run/validation before production execution.
7. Overly broad tags causing unintended production changes.

## Do / Don't Examples
### 1. Idempotency
```text
Don't: rely on ad-hoc shell commands for core state management.
Do:    use idempotent Ansible modules with explicit desired state.
```

### 2. Secret Handling
```text
Don't: commit plaintext credentials in group_vars.
Do:    store secrets in Ansible Vault or external secret manager.
```

### 3. Privilege Scope
```text
Don't: set broad privilege escalation globally by default.
Do:    scope privilege escalation only to tasks that require it.
```

## Code Review Checklist for Ansible
- Are playbooks and roles idempotent and deterministic?
- Are inventory and variable scopes environment-safe and maintainable?
- Are secrets excluded from plaintext files and logs?
- Is privilege escalation minimal and justified?
- Are dry-run/check-mode and validation steps present?
- Are rollback/remediation paths documented for risky changes?

## Testing Guidance
- Run `ansible-lint` and syntax checks in CI.
- Run check mode/dry-run in non-production for high-impact changes.
- Test role/playbook behavior with representative inventory fixtures.
- Test secret handling and redaction behavior in logs.
- Test rollback/remediation procedures for critical automation paths.

## Override Notes
- Project-specific Ansible patterns may narrow implementation details, but
  idempotency, secret hygiene, least privilege, and environment isolation
  remain mandatory.
