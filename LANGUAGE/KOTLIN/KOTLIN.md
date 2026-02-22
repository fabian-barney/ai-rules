# KOTLIN

Guidance for AI agents implementing and reviewing Kotlin code.

## Scope
- Define baseline Kotlin rules for correctness, maintainability, and runtime
  safety.
- Apply this file for Kotlin code generation and review tasks.

## Semantic Dependencies
- Inherit cross-cutting constraints from `SECURITY/SECURITY.md`,
  `TEST/TEST.md`, and `CORE/LOGGING.md`.
- Inherit shared language constraints from `LANGUAGE/CONVENTIONS.md` and
  `LANGUAGE/READABILITY.md`.
- Framework/library-specific Kotlin docs may specialize API usage but must not
  weaken this baseline.

## Defaults
- Keep null-safety explicit and avoid defeating type guarantees.
- Prefer immutable data and explicit state transitions.
- Use coroutines for structured async/concurrent workflows.
- Keep exceptions specific and boundary mapping explicit.
- Keep domain logic separated from framework/runtime plumbing.

## Null-Safety and Type Contracts
- Model optionality with nullable types intentionally.
- Avoid `!!` unless a bounded invariant is proven and documented.
- Keep DTO/domain nullability contracts explicit and consistent.
- Prefer sealed/value types for constrained domain state where suitable.

## Coroutines and Concurrency Rules
- Use structured concurrency with explicit parent scope ownership.
- Avoid `GlobalScope` for application business workflows.
- Keep dispatcher selection explicit for IO/CPU-sensitive paths.
- Propagate cancellation signals and timeouts for external calls.
- Prevent shared mutable-state races with explicit synchronization.

## Exception and Resource Handling
- Throw/map specific exceptions with actionable context.
- Do not swallow exceptions silently.
- Use `use {}` and lifecycle-aware resource management for deterministic cleanup.
- Preserve cause chains when wrapping exceptions at boundaries.

## API and Module Design
- Keep module/package boundaries cohesive and dependency direction explicit.
- Keep extension functions focused and avoid hidden side effects.
- Keep companion objects/static-like usage minimal and intentional.
- Keep configuration and secrets outside code.

## High-Risk Pitfalls
1. Frequent `!!` usage that bypasses null-safety contracts.
2. Unstructured coroutine launching with lifecycle leaks.
3. Blocking calls inside coroutine contexts without dispatcher isolation.
4. Catch-all exceptions that hide root causes.
5. Mutable shared state with no concurrency strategy.
6. Framework-coupled domain logic reducing testability.
7. Resource leaks due to missing scoped cleanup.

## Do / Don't Examples
### 1. Null Safety
```text
Don't: rely on `!!` in routine business logic.
Do:    model nullable intent explicitly and validate boundaries.
```

### 2. Coroutine Scope
```text
Don't: launch business-critical coroutines in `GlobalScope`.
Do:    use structured scopes with explicit lifecycle ownership.
```

### 3. Cancellation
```text
Don't: ignore cancellation and timeout behavior for external calls.
Do:    propagate cancellation and set explicit timeout boundaries.
```

## Code Review Checklist for Kotlin
- Are nullability contracts explicit and consistently enforced?
- Are coroutine scopes structured with clear lifecycle ownership?
- Are dispatcher usage and blocking behavior safe?
- Are exception mappings specific and non-silent?
- Are module boundaries cohesive and maintainable?
- Are dependencies/configuration/secrets handled safely?

## Testing Guidance
- Test nullability and boundary validation behavior.
- Test coroutine cancellation/timeout behavior under failure.
- Test concurrency-sensitive code for race/leak risks.
- Test exception mapping and root-cause preservation.
- Add regression tests for previously observed bug classes.

## Override Notes
- Project-specific Kotlin conventions may add stricter patterns, but null-safety
  integrity, structured concurrency, explicit error handling, and boundary
  cohesion remain mandatory.
