# DORA

Engineering policy guidance for AI agents on EU Digital Operational Resilience
Act (DORA) relevant software and operations controls.

This is engineering policy guidance, not legal advice.
Consult qualified legal counsel for final legal decisions.

## Scope
- Define baseline engineering controls that support DORA-style ICT risk and
  resilience obligations.
- Apply this file when systems support or integrate with regulated financial
  operations where DORA controls are in scope.

## Semantic Dependencies
- Inherit compliance baseline from `COMPLIANCE/COMPLIANCE.md`.
- Inherit security controls from `SECURITY/SECURITY.md`.
- Inherit infrastructure and CI/CD controls from `INFRASTRUCTURE/**` and
  `CI-CD/**`.
- Inherit logging/observability controls from `CORE/LOGGING.md`.

## Defaults
- Keep ICT risk ownership explicit for critical business services.
- Keep resilience-by-design controls integrated in delivery pipelines.
- Keep incident handling, communication, and evidence trails auditable.
- Keep third-party ICT dependency risk continuously managed.
- Keep recovery and continuity controls validated through regular exercises.

## ICT Risk Management Controls
- Keep critical-service inventory and dependency mapping current.
- Track risk scenarios, mitigation owners, and remediation deadlines.
- Enforce change-management controls for high-impact ICT changes.
- Require bounded approval for temporary control exceptions.

## Incident Handling and Reporting Readiness
- Define incident severity classification tied to escalation obligations.
- Keep detection, triage, containment, and recovery workflows documented.
- Keep event timelines and decision logs suitable for reporting obligations.
- Ensure post-incident corrective actions are tracked to closure.

## Resilience Testing and Continuity
- Keep backup, restore, and failover objectives explicit.
- Run periodic resilience testing for critical services.
- Validate recovery time and recovery point assumptions against targets.
- Keep scenario tests for dependency outage and degradation events.

## Third-Party ICT Risk Controls
- Keep critical vendor/service dependency register current.
- Define fallback/exit strategies for high-criticality third-party services.
- Keep integration privileges scoped and auditable.
- Reassess third-party risk after major incidents or material changes.

## High-Risk Pitfalls
1. Critical ICT services without explicit risk/control ownership.
2. Incident workflows that do not produce auditable reporting evidence.
3. Recovery assumptions untested in realistic failure scenarios.
4. Third-party critical dependencies with no fallback strategy.
5. Change velocity that bypasses resilience and approval gates.
6. Risk exceptions granted with no expiry or remediation plan.
7. Post-incident actions not tracked through completion.

## Do / Don't Examples
### 1. Incident Evidence
```text
Don't: run major incident response with no structured decision logging.
Do:    keep timestamped incident timelines and remediation evidence.
```

### 2. Third-Party Dependency
```text
Don't: rely on a critical vendor with no continuity or exit planning.
Do:    maintain fallback and recovery strategy for critical dependencies.
```

### 3. Recovery Validation
```text
Don't: accept untested recovery objectives as operationally proven.
Do:    test restore/failover scenarios and record outcomes.
```

## Code Review Checklist for DORA Context
- Is critical-service and dependency ownership explicit?
- Are ICT risk controls and mitigation responsibilities documented?
- Are incident detection/response/reporting hooks operationally usable?
- Are continuity/recovery objectives explicit and tested?
- Are third-party ICT dependencies controlled with fallback/exit planning?
- Are risk exceptions bounded, approved, and tracked to closure?

## Testing Guidance
- Test incident-response workflows and reporting evidence capture.
- Test continuity/failover/restore exercises for critical services.
- Test dependency-outage scenarios for critical third-party integrations.
- Test CI/CD controls that enforce high-impact change gates.
- Test closure tracking for incident and resilience remediation actions.

## Override Notes
- Project or sector-specific regulation may be stricter; stricter policy always
  wins.
