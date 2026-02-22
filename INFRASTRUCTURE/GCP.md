# GCP

Guidance for AI agents implementing and reviewing Google Cloud Platform (GCP)
configuration and operations choices.

## Scope
- Define GCP-specific platform guardrails for secure and reliable operations.
- Apply this file when architecture or infrastructure changes materially affect
  GCP organization, folder, project, network, identity, data, or observability
  posture.

## Semantic Dependencies
- Inherit infrastructure baseline from `INFRASTRUCTURE/INFRASTRUCTURE.md`.
- Inherit IaC/process constraints from `INFRASTRUCTURE/INFRA_AS_CODE.md`.
- Inherit security/compliance constraints from `SECURITY/SECURITY.md` and
  `COMPLIANCE/COMPLIANCE.md`.
- Inherit CI/CD and workflow constraints from `CI-CD/CI-CD.md` and
  `CORE/VERSION_CONTROL_SYSTEM.md`.

## Defaults
- Use org/folder/project hierarchy for environment and ownership isolation.
- Apply least privilege IAM by default for users, service accounts, and groups.
- Keep network access private by default; expose public endpoints intentionally.
- Enable encryption and key-management controls for sensitive data paths.
- Keep labeling standards mandatory for ownership, environment, and cost center.

## Identity and Access Guardrails
- Prefer workload identity federation and short-lived credentials.
- Minimize broad primitive roles and high-privilege grants.
- Keep service-account key creation heavily restricted and monitored.
- Keep cross-project access explicit and least-privilege.

## Network and Data Protection
- Keep VPC/firewall intent explicit and least-open.
- Avoid broad ingress/egress rules unless explicitly justified.
- Keep sensitive services private behind controlled access layers.
- Enforce key-management and secret handling controls for regulated data paths.

## Logging, Audit, and Detection
- Keep audit logging enabled at required levels for critical services.
- Centralize logs/metrics for operational and security analysis.
- Enable detection/monitoring controls for threat and misconfiguration signals.
- Keep alert ownership and escalation routing explicit.

## Reliability and Operational Safety
- Keep multi-zone/region resilience assumptions explicit for critical workloads.
- Use staged rollout for risky platform changes.
- Keep backup/restore and disaster-recovery procedures tested.
- Monitor quotas, API limits, and scaling boundaries proactively.

## High-Risk Pitfalls
1. Weak project/folder isolation increasing blast radius.
2. Broad IAM grants and unmanaged service-account keys.
3. Overly permissive firewall/network defaults.
4. Missing audit logs or weak retention controls.
5. Unencrypted data paths or unmanaged key usage.
6. No tested backup/restore path for critical services.
7. Missing labels/ownership reducing governance and response quality.

## Do / Don't Examples
### 1. Identity
```text
Don't: rely on long-lived service-account keys for normal automation.
Do:    use workload identity federation and least-privilege IAM.
```

### 2. Network Exposure
```text
Don't: allow broad internet ingress by default.
Do:    scope firewall rules to explicit trusted sources and ports.
```

### 3. Auditability
```text
Don't: operate production projects without strong audit logging.
Do:    enable and retain audit logs with monitored alerting.
```

## Code Review Checklist for GCP
- Are org/folder/project boundaries explicit and safe?
- Are IAM assignments least-privilege and scoped correctly?
- Are service-account key controls restrictive and auditable?
- Are network controls least-open and intentional?
- Are encryption/key/secret controls configured correctly?
- Are logging/audit/detection controls enabled and useful?
- Are backup/restore and resilience assumptions explicit and testable?
- Are required ownership/cost labels present?

## Testing Guidance
- Validate IAM boundaries with least-privilege checks.
- Validate service-account key restrictions and monitoring controls.
- Validate network exposure and policy compliance with automated checks.
- Validate logging/audit collection and alert routes.
- Validate backup/restore drills for critical services.
- Validate rollout/rollback behavior for high-impact changes.

## Override Notes
- Project-specific GCP conventions may narrow implementation details, but
  least-privilege identity, private-by-default networking, auditability, and
  operational resilience controls remain mandatory.
