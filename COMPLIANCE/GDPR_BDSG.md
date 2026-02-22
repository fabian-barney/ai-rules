# GDPR_BDSG

Engineering policy guidance for AI agents on GDPR (DSGVO) and BDSG relevant
software decisions in EU/German contexts.

This is engineering policy guidance, not legal advice.
Consult qualified legal counsel for final legal decisions.

## Scope
- Define baseline engineering controls for personal-data handling aligned with
  GDPR/BDSG expectations.
- Apply this file when systems process, store, transfer, or log personal data
  (including pseudonymous identifiers where applicable).

## Semantic Dependencies
- Inherit compliance baseline from `COMPLIANCE/COMPLIANCE.md` and
  `COMPLIANCE/LICENSES.md`.
- Inherit security controls from `SECURITY/SECURITY.md`.
- Inherit logging/data-minimization constraints from `CORE/LOGGING.md`.
- Inherit testing and review constraints from `TEST/TEST.md` and
  `REVIEW/CODE_REVIEW.md`.

## Defaults
- Keep personal-data inventory explicit per system boundary.
- Apply purpose limitation and data minimization by default.
- Keep lawful basis mapping explicit for each processing purpose.
- Keep retention/deletion policies explicit and automatable.
- Keep privacy-relevant decisions and exceptions auditable.

## Data Classification and Collection Rules
- Classify data by sensitivity and purpose before implementation changes.
- Collect only fields required for current documented purpose.
- Avoid collecting special-category personal data unless explicitly justified and
  approved.
- Keep telemetry/analytics payloads free of unnecessary personal data.

## Lawful Basis and Transparency Hooks
- Map each processing purpose to a documented lawful basis.
- Keep user-facing disclosures aligned with actual processing behavior.
- Require explicit consent handling where consent is the legal basis.
- Keep consent capture, versioning, and withdrawal behavior traceable.

## Retention, Deletion, and Data Subject Rights
- Implement retention schedules with automatic expiry/deletion where possible.
- Support DSAR-relevant operations (access, correction, deletion,
  restriction/objection) through documented workflows.
- Keep deletion effective across primary data, replicas, caches, and derived
  stores where feasible.
- Keep legal-hold exceptions explicit, approved, and time-bounded.

## Access, Transfer, and Processor Controls
- Enforce least privilege for personal-data access paths.
- Keep processor/subprocessor usage documented and contract-aligned.
- Validate cross-border transfer constraints before enabling new data flows.
- Keep data exports/imports auditable and purpose-bound.

## DPIA and High-Risk Change Triggers
- Escalate to privacy/legal review when processing risk materially increases.
- Treat large-scale profiling, sensitive-category processing, or new
  surveillance-like capabilities as DPIA triggers.
- Block rollout of high-risk processing changes until required approvals exist.

## High-Risk Pitfalls
1. Collecting personal data without explicit purpose and lawful basis mapping.
2. Logging full personal payloads or persistent identifiers unnecessarily.
3. Retaining personal data indefinitely with no deletion path.
4. Building features that require consent but have no withdrawal path.
5. Shipping high-risk processing changes without DPIA/privacy review.
6. Cross-border transfer enablement without legal/compliance validation.
7. DSAR workflows that cannot locate or delete all relevant data copies.

## Do / Don't Examples
### 1. Data Minimization
```text
Don't: add broad personal profile fields "just in case" for future analytics.
Do:    collect only fields required for documented current purpose.
```

### 2. Logging
```text
Don't: log full request bodies containing personal data by default.
Do:    log minimal identifiers and redact sensitive fields.
```

### 3. Retention
```text
Don't: keep personal records forever with no retention schedule.
Do:    enforce explicit retention TTLs and verified deletion workflows.
```

## Code Review Checklist for GDPR/BDSG
- Is personal-data processing purpose explicit and necessary?
- Is lawful basis mapping documented for each processing purpose?
- Are minimization and redaction controls applied to logs/telemetry?
- Are retention/deletion rules explicit and technically enforceable?
- Are DSAR-relevant data access/correction/deletion workflows feasible?
- Are cross-border transfer implications identified and reviewed?
- Were high-risk changes escalated for privacy/legal review where required?

## Testing Guidance
- Test that personal-data redaction rules apply to logs/errors/telemetry.
- Test retention and deletion workflows, including replica/cache behavior.
- Test consent-capture and withdrawal flows where consent is required.
- Test DSAR workflows for data lookup, export, correction, and deletion paths.
- Test authorization boundaries for personal-data access APIs and tooling.

## Override Notes
- Project-specific privacy policies may be stricter; stricter policy always
  wins.
