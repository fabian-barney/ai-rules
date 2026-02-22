# CLOUDFORMATION

Guidance for AI agents implementing and reviewing AWS CloudFormation
infrastructure changes.

## Scope
- Define CloudFormation-specific rules for safe and reproducible AWS
  infrastructure delivery.
- Apply this file to CloudFormation templates, stacks, change sets, and related
  deployment workflows.

## Semantic Dependencies
- Inherit IaC baseline from `INFRASTRUCTURE/INFRA_AS_CODE.md`.
- Inherit security and compliance constraints from `SECURITY/SECURITY.md` and
  `COMPLIANCE/COMPLIANCE.md`.
- Inherit CI and workflow constraints from `CI-CD/CI-CD.md` and
  `CORE/VERSION_CONTROL_SYSTEM.md`.

## Defaults
- Keep templates declarative, parameterized, and environment-aware.
- Keep stack boundaries cohesive and ownership explicit.
- Prefer nested stacks/modules for reuse over copy-pasted large templates.
- Keep change sets as the default review path before stack updates.
- Keep stack policies/termination protection enabled for critical stacks.

## Change Set and Update Rules
- Create and review change sets before executing updates.
- Block execution when change sets show unexpected replacement/deletion impact.
- Keep production updates gated with explicit approvals.
- Keep rollback behavior enabled unless a bounded exception is documented.

## Stack Governance
- Keep stack naming conventions stable across environments.
- Separate environments/accounts to reduce blast radius.
- Keep resource tags/metadata mandatory for ownership and cost governance.
- Avoid unmanaged/manual changes outside stack control.

## Drift, Import, and Migration
- Run drift detection for critical stacks regularly.
- Use resource import and stack refactor workflows deliberately and documented.
- Validate replacement impact before updates on stateful resources.
- Keep migration and rollback steps explicit for high-impact changes.

## Secrets and Access
- Do not hardcode secrets in templates or parameter defaults.
- Retrieve secrets through secure references/services.
- Keep IAM permissions least-privilege for deploy roles.
- Keep stack outputs free of sensitive data where possible.

## High-Risk Pitfalls
1. Updating stacks without reviewed change sets.
2. Ignoring replacement impact on stateful resources.
3. Missing termination protection on critical stacks.
4. Template-embedded secrets or sensitive outputs.
5. Manual console edits creating unmanaged drift.
6. Broad deploy-role permissions with weak guardrails.
7. Oversized templates with unclear ownership boundaries.

## Do / Don't Examples
### 1. Change-Set Discipline
```text
Don't: deploy stack updates directly with no change-set review.
Do:    review change set and execute only expected changes.
```

### 2. Stack Protection
```text
Don't: allow critical stacks to be deleted accidentally.
Do:    enable termination protection and restrictive stack policies.
```

### 3. Secret Handling
```text
Don't: store secret values directly in template parameters/defaults.
Do:    reference secrets from managed secret services.
```

## Code Review Checklist for CloudFormation
- Is change-set evidence provided and reviewed?
- Are destructive/replacement actions expected and justified?
- Are stack policies/termination protection configured correctly?
- Are environment/account boundaries explicit and safe?
- Are secrets absent from templates/outputs and deploy logs?
- Are drift, migration, and rollback paths explicit?

## Testing Guidance
- Validate templates syntactically and semantically before deployment.
- Generate and review change sets in CI/staging prior to production execution.
- Test stack updates and rollback behavior in non-production.
- Test drift detection and reconciliation for critical stacks.
- Test migration/import paths for complex stack evolution.

## Override Notes
- Project-specific CloudFormation patterns may narrow implementation details,
  but change-set review discipline, stack protection, least privilege, and
  secret hygiene remain mandatory.
