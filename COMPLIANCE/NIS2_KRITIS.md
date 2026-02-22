# NIS2_KRITIS

Engineering policy guidance for AI agents on NIS2-oriented controls and German
KRITIS-context operational obligations.

This is engineering policy guidance, not legal advice.
Consult qualified legal counsel for final legal decisions.

## Scope
- Define baseline engineering controls that support NIS2-style cybersecurity and
  resilience obligations in EU/German critical-operations contexts.
- Apply this file when systems are in scope for heightened operational-security
  and incident-reporting expectations.

## Semantic Dependencies
- Inherit compliance baseline from `COMPLIANCE/COMPLIANCE.md`.
- Inherit security controls from `SECURITY/SECURITY.md`.
- Inherit infrastructure and CI/CD controls from `INFRASTRUCTURE/**` and
  `CI-CD/**`.
- Inherit logging/observability constraints from `CORE/LOGGING.md`.

## Defaults
- Keep security governance ownership explicit for critical services.
- Keep risk assessment and mitigation evidence current and auditable.
- Keep incident detection, classification, and escalation paths documented.
- Keep supply-chain and third-party dependency risk controls active.
- Keep business-continuity and recovery assumptions tested.

## Governance and Risk Controls
- Maintain current asset/service inventory for in-scope systems.
- Maintain risk register entries for major service and dependency risks.
- Track remediation actions with owner and due date.
- Require explicit approval for temporary risk exceptions and expiry dates.

## Incident Readiness and Reporting Hooks
- Define incident severity model and escalation ownership.
- Keep incident triage procedures and responder runbooks available.
- Keep timestamped evidence collection ready for forensic/regulatory needs.
- Ensure incident classification supports required reporting timelines.
- Block unresolved critical incident states from silent production progression.

## Supply Chain and Dependency Governance
- Keep dependency provenance and vulnerability scanning in CI.
- Evaluate third-party provider criticality and failure impact regularly.
- Keep third-party access and integration scopes least-privilege.
- Keep fallback and substitution plans for critical third-party services.

## Resilience, Continuity, and Recovery
- Keep backup, restore, and recovery objectives explicit for critical systems.
- Keep disaster-recovery exercises and failover validation periodic.
- Avoid single points of failure in infrastructure/control planes where feasible.
- Keep change management gates strong for critical production paths.

## High-Risk Pitfalls
1. No explicit ownership for operational cybersecurity controls.
2. Incident processes that cannot support required reporting timelines.
3. Missing evidence trails for incident decisions and remediation.
4. Critical third-party dependencies without fallback strategy.
5. Untracked risk exceptions with no expiry/approval.
6. Recovery plans that are documented but never tested.
7. Security controls bypassed for urgent changes without post-control closure.

## Do / Don't Examples
### 1. Incident Readiness
```text
Don't: rely on ad-hoc chat coordination with no severity/escalation model.
Do:    maintain documented triage/severity workflow and response ownership.
```

### 2. Third-Party Risk
```text
Don't: treat critical vendor dependencies as low-risk by default.
Do:    assess vendor criticality and define fallback/continuity paths.
```

### 3. Recovery Validation
```text
Don't: assume backups are sufficient without restore drills.
Do:    run and document periodic restore/failover tests.
```

## Code Review Checklist for NIS2/KRITIS Context
- Are critical-service ownership and risk controls explicit?
- Are incident detection/classification/escalation paths defined and usable?
- Are logging/evidence trails sufficient for incident reporting obligations?
- Are supply-chain and third-party dependency controls active?
- Are recovery objectives and tested continuity paths documented?
- Are temporary risk exceptions approved, bounded, and tracked to closure?

## Testing Guidance
- Test incident-detection and escalation workflows end-to-end.
- Test incident evidence capture and timeline reconstruction ability.
- Test dependency-failure scenarios and fallback behavior.
- Test backup/restore and failover for critical services.
- Test CI security/vulnerability gates for dependency and pipeline changes.

## Override Notes
- Project or sector-specific regulation may be stricter; stricter policy always
  wins.
