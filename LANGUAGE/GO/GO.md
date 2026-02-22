# GO

Guidance for AI agents implementing and reviewing Go code.

## Scope
- Define baseline Go rules for correctness, maintainability, and production
  safety.
- Apply this file for Go code generation and review tasks.

## Semantic Dependencies
- Inherit cross-cutting constraints from `SECURITY/SECURITY.md`,
  `TEST/TEST.md`, and `CORE/LOGGING.md`.
- Inherit shared language constraints from `LANGUAGE/CONVENTIONS.md` and
  `LANGUAGE/READABILITY.md`.
- Framework/library-specific Go docs may specialize API usage but must not
  weaken this baseline.

## Defaults
- Prefer simple, explicit code over abstraction-heavy indirection.
- Keep package boundaries cohesive with clear ownership.
- Return errors explicitly and handle them near boundaries.
- Use `context.Context` consistently for request-scoped cancellation/timeouts.
- Keep dependencies minimal and maintainable.

## API and Package Design
- Keep exported APIs small and stable.
- Use interfaces at boundaries where polymorphism is required, not everywhere.
- Keep package init logic minimal and side-effect free.
- Avoid cyclic package dependencies and hidden global coupling.

## Error Handling and Resource Safety
- Return rich, contextual errors and preserve root cause context.
- Do not ignore returned errors.
- Keep panic usage exceptional (programmer errors/unrecoverable states).
- Close resources deterministically (`defer` with explicit error handling where
  needed).

## Concurrency and Context Rules
- Prefer message passing/channel patterns over shared mutable state when
  practical.
- Guard shared state explicitly when unavoidable.
- Avoid goroutine leaks; ensure cancellation and completion paths exist.
- Pass `context.Context` as first parameter for request-scoped operations.
- Keep timeouts/deadlines explicit for external IO.

## High-Risk Pitfalls
1. Ignoring errors and assuming happy-path behavior.
2. Package-level globals introducing hidden mutable state.
3. Goroutine leaks due to missing cancellation/closure conditions.
4. Blocking operations with no context timeout.
5. Premature interface abstraction that reduces readability.
6. Panic-driven control flow in normal runtime paths.
7. Resource leaks due to missing close/defer handling.

## Do / Don't Examples
### 1. Error Handling
```text
Don't: discard returned error values.
Do:    handle or propagate errors with context.
```

### 2. Context Discipline
```text
Don't: perform network/database calls without timeout or cancellation context.
Do:    pass context and enforce deadlines at external boundaries.
```

### 3. Concurrency Safety
```text
Don't: spawn goroutines with no lifecycle control.
Do:    define clear cancellation and completion paths.
```

## Code Review Checklist for Go
- Are package boundaries clear and cohesive?
- Are errors handled explicitly with useful context?
- Are contexts/timeouts/cancellation used correctly for IO paths?
- Is concurrency free from obvious races/leaks/deadlocks?
- Are globals and shared mutable state minimized?
- Are dependencies justified and minimal?

## Testing Guidance
- Test error-path behavior and wrapped error context.
- Test context cancellation/timeout behavior for IO-bound operations.
- Test concurrency-sensitive code for races and goroutine leaks.
- Test package boundary contracts with integration-level coverage where needed.
- Add regression tests for previous bug classes.

## Override Notes
- Project-specific Go conventions may add stricter patterns, but explicit error
  handling, context discipline, concurrency safety, and deterministic resource
  management remain mandatory.
