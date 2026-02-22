# SWIFT

Guidance for AI agents implementing and reviewing Swift code.

## Scope
- Define baseline Swift rules for correctness, maintainability, and runtime
  safety.
- Apply this file for Swift code generation and review tasks.

## Semantic Dependencies
- Inherit cross-cutting constraints from `SECURITY/SECURITY.md`,
  `TEST/TEST.md`, and `CORE/LOGGING.md`.
- Inherit shared language constraints from `LANGUAGE/CONVENTIONS.md` and
  `LANGUAGE/READABILITY.md`.
- Framework/library-specific Swift docs may specialize API usage but must not
  weaken this baseline.

## Defaults
- Prefer value semantics (`struct`, immutable state) when practical.
- Keep optional handling explicit and avoid force-unwrapping in runtime paths.
- Use Swift concurrency (`async`/`await`, actors) with explicit isolation.
- Keep API contracts and error paths explicit and testable.
- Keep domain logic separate from UI/framework lifecycle code.

## Optionals and Type Contracts
- Model absence with optionals intentionally.
- Avoid `!` unless a bounded invariant is proven and documented.
- Keep boundary parsing/validation explicit for external input.
- Use enums/value objects for constrained domain states where suitable.

## Concurrency and Isolation Rules
- Use structured concurrency for async workflows.
- Use actors or other safe synchronization for shared mutable state.
- Avoid blocking the main actor with long-running work.
- Keep cancellation and timeout behavior explicit for external IO.
- Keep task ownership/lifecycle clear to avoid orphaned async work.

## Error and Resource Handling
- Throw/map specific errors with actionable context.
- Do not suppress errors silently.
- Use deterministic cleanup for files/network/resources.
- Preserve cause context when mapping low-level failures.

## API and Module Boundaries
- Keep module boundaries cohesive with explicit dependency direction.
- Keep protocol abstractions purposeful, not premature.
- Keep side effects isolated at service/adaptor boundaries.
- Keep configuration/secrets outside code.

## High-Risk Pitfalls
1. Force-unwrapping optionals in normal runtime paths.
2. Unstructured tasks with unclear cancellation/lifecycle.
3. Data races from shared mutable state outside safe isolation.
4. Blocking main actor with network/disk-heavy operations.
5. Broad error handling that hides root causes.
6. Framework-coupled domain logic reducing testability.
7. Resource leaks from weak cleanup discipline.

## Do / Don't Examples
### 1. Optional Safety
```text
Don't: force-unwrap values from external input.
Do:    validate and handle optionals explicitly at boundaries.
```

### 2. Concurrency Isolation
```text
Don't: mutate shared state from multiple tasks without protection.
Do:    use actors or explicit synchronization boundaries.
```

### 3. UI/Main Thread Safety
```text
Don't: perform long IO work on the main actor.
Do:    move heavy work off main actor and update UI intentionally.
```

## Code Review Checklist for Swift
- Are optional/type contracts explicit and safe?
- Is async/task lifecycle structured with clear cancellation behavior?
- Is shared mutable state isolated safely (actors/synchronization)?
- Are error paths explicit and context-rich?
- Are domain and framework/UI boundaries cleanly separated?
- Are dependencies/configuration/secrets handled safely?

## Testing Guidance
- Test optional-handling and boundary validation behavior.
- Test async cancellation/timeout behavior under failure.
- Test actor/isolation-sensitive code for race/leak risks.
- Test error mapping and root-cause preservation.
- Add regression tests for previously observed bug classes.

## Override Notes
- Project-specific Swift conventions may add stricter patterns, but optional
  safety, structured concurrency/isolation, explicit error handling, and clear
  boundary separation remain mandatory.
