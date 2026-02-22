# PYTHON

Guidance for AI agents implementing and reviewing Python code.

## Scope
- Define baseline Python rules for correctness, maintainability, and production
  safety.
- Apply this file for Python code generation and review tasks.

## Semantic Dependencies
- Inherit cross-cutting constraints from `SECURITY/SECURITY.md`,
  `TEST/TEST.md`, and `CORE/LOGGING.md`.
- Inherit shared language constraints from `LANGUAGE/CONVENTIONS.md` and
  `LANGUAGE/READABILITY.md`.
- Framework/library-specific Python docs may specialize API usage but must not
  weaken this baseline.

## Defaults
- Prefer explicit, readable code over clever one-liners.
- Use type hints for public APIs and complex internal boundaries.
- Keep module boundaries cohesive and side effects explicit.
- Use virtual environments and pinned dependencies for reproducibility.
- Validate external input at boundaries and fail fast with clear errors.

## Typing and API Design
- Type public function/method signatures and return values.
- Use domain-specific types/dataclasses for structured business data.
- Avoid broad `Any` usage unless explicitly justified.
- Keep `Optional`/`None` handling explicit at call boundaries.

## Error Handling and Resource Safety
- Raise specific exceptions with actionable context.
- Do not swallow exceptions silently.
- Use context managers for files/sockets/transactions/locks.
- Preserve root cause context when mapping exceptions at boundaries.

## State, Concurrency, and Async Rules
- Avoid shared mutable global state by default.
- Keep thread/process safety explicit when concurrency is used.
- Use `async`/`await` for IO-bound concurrency, not CPU-bound work.
- Keep async cancellation and timeout behavior explicit for external calls.

## Packaging and Imports
- Keep import paths stable and avoid circular dependencies.
- Keep configuration separate from runtime/business logic.
- Keep dependency surface minimal and justified.
- Keep scripts/entrypoints thin and delegate behavior to testable modules.

## High-Risk Pitfalls
1. Untyped public APIs that hide contract drift.
2. Catch-all exception handlers that mask production failures.
3. Hidden side effects during import/module initialization.
4. Shared mutable globals causing concurrency bugs.
5. Mixing blocking calls into async request paths.
6. Implicit `None` handling that creates runtime failures.
7. Unpinned dependencies causing non-deterministic behavior.

## Do / Don't Examples
### 1. Exception Handling
```text
Don't: catch Exception and continue silently.
Do:    catch specific exception types and preserve context.
```

### 2. Typing
```text
Don't: expose untyped public service interfaces.
Do:    type inputs/outputs for stable contracts and review clarity.
```

### 3. Async Safety
```text
Don't: call blocking network/file APIs inside async request handlers.
Do:    use async-compatible clients or isolate blocking work explicitly.
```

## Code Review Checklist for Python
- Are public APIs typed and contracts explicit?
- Is exception handling specific and non-silent?
- Are resource lifecycles managed safely (context managers/timeouts)?
- Are concurrency/async assumptions explicit and safe?
- Are module boundaries cohesive with minimal side effects?
- Are dependency additions justified and pinned?

## Testing Guidance
- Test typed API contracts and boundary validation behavior.
- Test exception mapping and error-path outcomes.
- Test concurrency/async paths for timeout/cancellation/race conditions.
- Test configuration parsing and environment-boundary logic.
- Add regression tests for previously observed bug classes.

## Override Notes
- Project-specific Python conventions may add stricter patterns, but explicit
  typing, boundary validation, safe error handling, and deterministic dependency
  behavior remain mandatory.
