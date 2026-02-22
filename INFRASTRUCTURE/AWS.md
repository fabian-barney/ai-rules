# AWS

Guidance for AI agents implementing and reviewing AWS platform configuration
and operations choices.

## Scope
- Define AWS-specific platform guardrails for secure and reliable operations.
- Apply this file when architecture or infrastructure changes materially affect
  AWS account, network, identity, data, or observability posture.

## Semantic Dependencies
- Inherit infrastructure baseline from `INFRASTRUCTURE/INFRASTRUCTURE.md`.
- Inherit IaC/process constraints from `INFRASTRUCTURE/INFRA_AS_CODE.md`.
- Inherit security/compliance constraints from `SECURITY/SECURITY.md` and
  `COMPLIANCE/COMPLIANCE.md`.
- Inherit CI/CD and workflow constraints from `CI-CD/CI-CD.md` and
  `CORE/VERSION_CONTROL_SYSTEM.md`.

## Defaults
- Use multi-account strategies for environment and blast-radius isolation.
- Apply least privilege IAM by default for users, roles, and services.
- Keep network access private by default; expose public endpoints intentionally.
- Enable encryption in transit and at rest for sensitive data paths.
- Keep tagging standards mandatory for ownership, environment, and cost center.

## Identity and Access Guardrails
- Prefer role assumption and short-lived credentials over long-lived static
  access keys.
- Keep human admin access behind strong MFA and audit controls.
- Restrict cross-account trust with explicit conditions and least privilege.
- Review wildcard permissions (`*`) and high-risk actions as exceptions only.

## Network and Data Protection
- Keep VPC, subnet, and security-group intent explicit and least-open.
- Avoid unrestricted ingress/egress rules unless explicitly justified.
- Keep data services private and fronted by controlled access layers.
- Enforce key-management policy and rotation where required.

## Logging, Audit, and Detection
- Keep account/org audit logs enabled and protected from tampering.
- Keep service/application logs centralized and queryable.
- Enable detection/monitoring controls for threat and misconfiguration events.
- Keep alert routing and escalation ownership explicit.

## Reliability and Operational Safety
- Keep region/AZ resilience assumptions explicit for critical workloads.
- Use staged rollout for risky platform changes.
- Keep backup/restore and disaster-recovery procedures tested.
- Keep quotas/limits and scaling boundaries monitored proactively.

## High-Risk Pitfalls
1. Single-account designs with no blast-radius isolation.
2. Broad IAM permissions and unused long-lived access keys.
3. Publicly exposed resources by default with weak ingress controls.
4. Missing audit logs or mutable log destinations.
5. Unencrypted data stores or unmanaged key usage.
6. No tested backup/restore path for critical data/services.
7. Missing tagging/ownership causing operational blind spots.

## Do / Don't Examples
### 1. Identity
```text
Don't: rely on static admin access keys for automation.
Do:    use assumed roles and short-lived credentials.
```

### 2. Network Exposure
```text
Don't: open broad inbound CIDR ranges by default.
Do:    scope security-group rules to explicit trusted sources.
```

### 3. Auditability
```text
Don't: run critical environments without centralized audit logging.
Do:    enable immutable audit trails with monitored alerting.
```

## Code Review Checklist for AWS
- Are account/environment isolation boundaries explicit and safe?
- Are IAM permissions least-privilege with no unjustified wildcards?
- Are network controls least-open and intentional?
- Are encryption/key-management requirements satisfied?
- Are logging/audit/detection controls enabled and tamper-resistant?
- Are backup/restore and resilience assumptions explicit and testable?
- Are required ownership/cost tags present?

## Testing Guidance
- Validate IAM policies/role trust with least-privilege tests.
- Validate network exposure with automated policy/security checks.
- Validate audit/logging pipelines and alert routes.
- Validate backup/restore drills for critical services.
- Validate staged rollout and rollback procedures for high-impact changes.

## Override Notes
- Project-specific AWS conventions may narrow implementation details, but
  least-privilege identity, private-by-default networking, auditability, and
  operational resilience controls remain mandatory.
