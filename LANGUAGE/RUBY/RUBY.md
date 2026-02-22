# RUBY

Guidance for AI agents implementing and reviewing Ruby code.

## Scope
- Define baseline Ruby rules for correctness, maintainability, and production
  safety.
- Apply this file for Ruby code generation and review tasks.

## Semantic Dependencies
- Inherit cross-cutting constraints from `SECURITY/SECURITY.md`,
  `TEST/TEST.md`, and `CORE/LOGGING.md`.
- Inherit shared language constraints from `LANGUAGE/CONVENTIONS.md` and
  `LANGUAGE/READABILITY.md`.
- Framework/library-specific Ruby docs may specialize API usage but must not
  weaken this baseline.

## Defaults
- Prefer explicit object contracts over ad-hoc dynamic behavior.
- Keep domain logic separate from framework/transport concerns.
- Keep side effects explicit and isolated to boundary layers.
- Keep dependency usage minimal and maintainable.
- Validate external input at boundaries before domain operations.

## API and Object Design
- Keep class/module responsibilities cohesive.
- Use explicit method arguments and return expectations for critical paths.
- Avoid metaprogramming for core business logic when plain Ruby is clearer.
- Keep mutation boundaries intentional and documented.

## Error Handling and Reliability
- Raise/map specific exceptions with contextual information.
- Do not rescue broad exceptions without bounded rationale.
- Keep retries/timeouts explicit for external dependencies.
- Preserve root cause context when mapping errors at boundaries.

## State and Side-Effect Control
- Avoid hidden global state and implicit mutable singletons.
- Keep transactional boundaries explicit for stateful operations.
- Keep IO operations at service/adaptor boundaries for testability.
- Keep logging redacted and purpose-minimal for sensitive fields.

## High-Risk Pitfalls
1. Overuse of metaprogramming that obscures behavior contracts.
2. Broad `rescue` blocks that hide production failures.
3. Implicit mutable global/singleton state causing order-dependent bugs.
4. Framework-coupled domain logic reducing testability.
5. Missing timeout/retry boundaries for external dependencies.
6. Unvalidated input reaching domain/persistence layers.
7. Excessive gem additions with weak maintenance posture.

## Do / Don't Examples
### 1. Exception Scope
```text
Don't: rescue StandardError broadly and continue silently.
Do:    rescue specific exceptions and preserve context.
```

### 2. Metaprogramming
```text
Don't: use dynamic magic for core business rules by default.
Do:    prefer explicit methods when behavior clarity matters.
```

### 3. Side Effects
```text
Don't: scatter external IO side effects through domain objects.
Do:    isolate IO at clear service/adaptor boundaries.
```

## Code Review Checklist for Ruby
- Are object/module responsibilities cohesive and clear?
- Are exception boundaries specific and non-silent?
- Are side effects and mutable state explicit and bounded?
- Are external dependency calls bounded by timeout/retry strategy?
- Are boundary validations present for external inputs?
- Are new gems justified, maintained, and compliant?

## Testing Guidance
- Test boundary validation and normalization behavior.
- Test exception mapping and failure-path outcomes.
- Test timeout/retry behavior for external dependencies.
- Test state/transaction boundaries for consistency guarantees.
- Add regression tests for previously observed bug classes.

## Override Notes
- Project-specific Ruby conventions may add stricter patterns, but explicit
  contracts, bounded error handling, clear side-effect boundaries, and boundary
  validation remain mandatory.
