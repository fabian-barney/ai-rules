# RUST

Guidance for AI agents implementing and reviewing Rust code.

## Scope
- Define baseline Rust rules for correctness, maintainability, and runtime
  safety.
- Apply this file for Rust code generation and review tasks.

## Semantic Dependencies
- Inherit cross-cutting constraints from `SECURITY/SECURITY.md`,
  `TEST/TEST.md`, and `CORE/LOGGING.md`.
- Inherit shared language constraints from `LANGUAGE/CONVENTIONS.md` and
  `LANGUAGE/READABILITY.md`.
- Framework/library-specific Rust docs may specialize API usage but must not
  weaken this baseline.

## Defaults
- Prefer explicit ownership and lifetimes over workaround cloning.
- Keep type/state invariants encoded in types where practical.
- Use `Result`/`Option` explicitly and avoid panic-driven normal flow.
- Keep `unsafe` usage minimal, justified, and encapsulated.
- Keep public API surfaces small and stable.

## Ownership and API Design
- Design APIs that communicate ownership/borrowing intent clearly.
- Avoid unnecessary `clone()` in hot paths; address lifetime design first.
- Keep module boundaries cohesive and explicit.
- Keep trait abstractions purposeful, not premature.

## Error Handling and Panics
- Use domain-specific error enums/structs with contextual information.
- Propagate recoverable failures via `Result`.
- Reserve `panic!` for unrecoverable programmer invariants.
- Preserve source context when mapping lower-level errors.

## Async and Concurrency Rules
- Keep async boundaries explicit and runtime-aware.
- Avoid blocking operations on async executors.
- Keep shared mutable state synchronized and minimized.
- Use channels/message passing where it improves clarity and safety.
- Keep cancellation/timeout behavior explicit for external IO.

## Unsafe and FFI Boundaries
- Isolate `unsafe` blocks behind safe APIs and documented invariants.
- Keep FFI boundary contracts explicit about ownership/lifetime/thread safety.
- Add targeted tests around unsafe-critical behavior.
- Review unsafe code with extra scrutiny in PRs.

## High-Risk Pitfalls
1. Excess cloning to bypass ownership design issues.
2. Panic-based control flow in recoverable runtime paths.
3. Async code using blocking operations on executor threads.
4. Hidden invariants in `unsafe` code with no safety docs/tests.
5. Overcomplicated trait abstractions reducing readability.
6. Error types that lose root-cause context.
7. Unbounded concurrency without backpressure/timeouts.

## Do / Don't Examples
### 1. Error Handling
```text
Don't: use `unwrap()` in normal service runtime paths.
Do:    propagate recoverable failures using contextual `Result` errors.
```

### 2. Unsafe Isolation
```text
Don't: scatter unsafe blocks across business logic.
Do:    isolate unsafe code behind small reviewed safe abstractions.
```

### 3. Async Safety
```text
Don't: run blocking IO directly inside async tasks.
Do:    use async-compatible clients or isolate blocking work explicitly.
```

## Code Review Checklist for Rust
- Are ownership/lifetime contracts explicit and readable?
- Are error paths contextual and panic usage bounded?
- Are async and blocking behaviors correctly separated?
- Is unsafe/FFI code minimal, documented, and tested?
- Are module/trait boundaries cohesive and maintainable?
- Are concurrency limits/backpressure/cancellation explicit?

## Testing Guidance
- Test error-path behavior with contextual mapping.
- Test async timeout/cancellation and backpressure behavior.
- Test unsafe/FFI boundary invariants and failure modes.
- Test concurrency-sensitive code for race/deadlock/starvation risks.
- Add regression tests for previously observed bug classes.

## Override Notes
- Project-specific Rust conventions may add stricter patterns, but explicit
  ownership contracts, safe error handling, bounded unsafe usage, and async
  runtime safety remain mandatory.
