# EPRIVACY_TTDSG

Engineering policy guidance for AI agents on ePrivacy-oriented and TTDSG
controls for client-side storage and tracking technologies.

This is engineering policy guidance, not legal advice.
Consult qualified legal counsel for final legal decisions.

## Scope
- Define baseline engineering controls for cookie/device storage and
  communications-tracking behavior in EU/German contexts.
- Apply this file when implementing web/mobile telemetry, tracking,
  personalization, or client-side identifier storage.

## Semantic Dependencies
- Inherit compliance baseline from `COMPLIANCE/COMPLIANCE.md`.
- Inherit privacy controls from `COMPLIANCE/GDPR_BDSG.md`.
- Inherit security controls from `SECURITY/SECURITY.md`.
- Inherit logging constraints from `CORE/LOGGING.md`.

## Defaults
- Disable non-essential tracking/storage by default until valid consent exists.
- Keep consent categories explicit and purpose-bound.
- Keep telemetry payloads minimal and privacy-preserving.
- Keep consent state auditable with versioned policy metadata.
- Keep withdrawal/revocation behavior equivalent to grant behavior.

## Consent and Purpose Controls
- Gate non-essential cookies/storage and similar tracking technologies behind
  explicit consent where required.
- Keep strict separation between essential and non-essential categories.
- Keep consent prompts clear, non-deceptive, and purpose-specific.
- Prevent implicit bundling of unrelated consent purposes.

## Client-Side Identifier and Storage Rules
- Minimize persistent identifiers and storage duration.
- Avoid hidden fingerprinting-like techniques without explicit legal approval.
- Rotate or scope identifiers where feasible to reduce tracking surface.
- Keep client-side storage contents free of unnecessary personal data.

## Telemetry and Analytics Boundaries
- Use aggregated/anonymized data where possible for analytics.
- Avoid transmitting raw user-content or sensitive fields in telemetry.
- Keep third-party analytics integrations bounded and configurable by consent
  state.
- Ensure consent state is enforced across all telemetry emitters.

## Revocation, Retention, and Auditability
- Implement immediate effect for consent withdrawal on non-essential tracking.
- Delete or disable non-essential identifiers/tokens upon revocation where
  feasible.
- Keep retention windows explicit for tracking-related data.
- Keep consent records and policy-version linkage auditable.

## High-Risk Pitfalls
1. Loading non-essential trackers before consent.
2. Treating pre-checked boxes or passive interaction as valid consent.
3. Storing long-lived identifiers with no necessity justification.
4. Sending sensitive user data into analytics payloads.
5. Ignoring withdrawal requests or delaying revocation effect.
6. Inconsistent consent enforcement across first-party and third-party scripts.
7. Missing consent audit evidence for policy/version in effect.

## Do / Don't Examples
### 1. Consent Gating
```text
Don't: initialize analytics scripts on initial page load by default.
Do:    delay non-essential trackers until valid consent is recorded.
```

### 2. Identifier Scope
```text
Don't: create persistent cross-context identifiers without necessity.
Do:    minimize identifier scope/lifetime and document purpose.
```

### 3. Withdrawal Handling
```text
Don't: keep tracking active after user revokes consent.
Do:    disable non-essential tracking immediately on revocation.
```

## Code Review Checklist for ePrivacy/TTDSG
- Are non-essential tracking/storage paths gated behind valid consent?
- Are consent categories clear, purpose-bound, and non-bundled?
- Are telemetry payloads minimized and free of sensitive content?
- Are third-party trackers/integrations fully consent-aware?
- Are identifier lifetime/scope choices justified and minimal?
- Are revocation and retention behaviors explicit and testable?
- Is consent evidence and policy-version linkage auditable?

## Testing Guidance
- Test tracker initialization behavior before and after consent.
- Test consent withdrawal effects across first-party and third-party scripts.
- Test category-specific consent toggles and enforcement consistency.
- Test telemetry payload redaction/minimization for sensitive fields.
- Test consent-record auditability including policy/version references.

## Override Notes
- Project or sector-specific regulation may be stricter; stricter policy always
  wins.
