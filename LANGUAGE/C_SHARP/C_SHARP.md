# C_SHARP

Guidance for AI agents implementing and reviewing C#/.NET code.

## Scope
- Define baseline C#/.NET rules for correctness, maintainability, and runtime
  safety.
- Apply this file for C#/.NET code generation and review tasks.

## Semantic Dependencies
- Inherit cross-cutting constraints from `SECURITY/SECURITY.md`,
  `TEST/TEST.md`, and `CORE/LOGGING.md`.
- Inherit shared language constraints from `LANGUAGE/CONVENTIONS.md` and
  `LANGUAGE/READABILITY.md`.
- Framework/library-specific C#/.NET docs may specialize API usage but must not
  weaken this baseline.

## Defaults
- Enable and respect nullable reference type warnings.
- Prefer explicit dependency injection over static/global dependencies.
- Keep async flows truly asynchronous end-to-end for IO paths.
- Keep exceptions specific, contextual, and centrally mapped at boundaries.
- Keep domain logic separated from transport/persistence concerns.

## Nullability and API Contracts
- Treat nullable annotations as part of API contract.
- Avoid returning null where an explicit optional/result model is clearer.
- Validate external inputs at boundaries and fail fast with clear errors.
- Keep DTO/domain model nullability intent explicit and consistent.

## Async, Concurrency, and Cancellation
- Use `async`/`await` for IO-bound work; avoid sync-over-async patterns.
- Propagate `CancellationToken` for request-scoped operations.
- Avoid blocking calls (`.Result`, `.Wait()`) in async paths.
- Keep shared mutable state synchronized and minimized.

## Exception and Resource Handling
- Throw and map specific exception types with actionable context.
- Do not swallow exceptions silently.
- Use `using`/`await using` for deterministic disposal.
- Preserve inner exception/root cause when wrapping.

## Architectural Boundaries
- Keep dependency direction explicit and testable.
- Keep service classes cohesive with one primary responsibility.
- Avoid static utility sprawl for domain behavior.
- Keep configuration and secrets out of code.

## High-Risk Pitfalls
1. Ignoring nullable warnings and relying on runtime null failures.
2. Blocking async request paths with `.Result`/`.Wait()`.
3. Missing cancellation propagation in external calls.
4. Catch-all exceptions that hide root causes.
5. Hidden dependencies through statics/singletons.
6. Blurred domain/infrastructure boundaries in one class.
7. Resource leaks from missing disposal patterns.

## Do / Don't Examples
### 1. Async Discipline
```text
Don't: call `.Result` inside an ASP.NET request path.
Do:    use `await` end-to-end and propagate cancellation.
```

### 2. Nullability
```text
Don't: disable nullable warnings to silence contract issues.
Do:    model nullable intent explicitly and validate boundaries.
```

### 3. Error Handling
```text
Don't: catch Exception and ignore it.
Do:    catch specific exceptions and preserve context.
```

## Code Review Checklist for C#/.NET
- Are nullable contracts explicit and respected?
- Are async paths non-blocking with proper cancellation flow?
- Are exception mappings specific and context-rich?
- Are disposal/resource lifecycles handled deterministically?
- Are architectural boundaries cohesive and explicit?
- Are dependency additions justified and maintainable?

## Testing Guidance
- Test nullability/validation behavior at API boundaries.
- Test async cancellation and timeout behavior for external IO.
- Test exception mapping and preserved root-cause details.
- Test concurrency-sensitive code for race/deadlock risks.
- Add regression tests for previously observed bug classes.

## Override Notes
- Project-specific C#/.NET conventions may add stricter patterns, but nullable
  contract integrity, async/cancellation safety, explicit error handling, and
  boundary separation remain mandatory.
