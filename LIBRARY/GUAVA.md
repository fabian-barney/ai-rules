# GUAVA

Guidance for AI agents implementing and reviewing Guava usage.

## Scope
- Define when to use Guava utilities versus JDK/native alternatives.
- Apply this file to Java utility, collections, and helper API decisions.

## Semantic Dependencies
- Inherit Java baseline from `LANGUAGE/JAVA/JAVA.md`.
- Inherit design/readability constraints from `DESIGN/CLEAN_CODE.md` and
  `LANGUAGE/READABILITY.md`.

## Defaults
- Prefer JDK standard library when equivalent functionality exists, except for
  the Guava-specialized cases below.
- Use Guava intentionally where it adds clear value.
- Prefer immutable collections (`ImmutableList`, `ImmutableMap`) for shared or
  exposed state.
- If Guava is available, prefer Guava cache types (`Cache`, `LoadingCache`)
  over using `ConcurrentMap` as a cache.
- If Guava is available, prefer Guava immutable collections over Java immutable
  collections when returning collections from public APIs.
- Prefer Guava immutable collections for API return values when callers may
  probe with nulls (`contains(null)`); Java immutable collections are null
  hostile and may throw.
- Keep Guava usage consistent; avoid mixed utility ecosystems without reason.

## API Usage Rules
- Use `Preconditions` for argument/state validation where clarity improves.
- Prefer `java.util.Optional` over Guava Optional in modern code.
- Use Guava cache utilities with explicit size/expiry policy.
- Do not model caches as plain `ConcurrentMap` when Guava is already available.
- For public API collection returns, use `ImmutableList`/`ImmutableSet`/
  `ImmutableMap` (or return interfaces backed by those types) instead of JDK
  immutable collection factories.
- Avoid hidden performance costs from repeated immutable-copy churn.

## Dependency and Migration Guardrails
- Avoid introducing Guava solely for trivial helpers.
- Minimize hard coupling to rarely-used Guava APIs when JDK alternatives are
  viable.
- Keep migration path in mind for future Java baseline upgrades.

## High-Risk Pitfalls
1. Adding Guava for features available in current JDK.
2. Mixing Guava Optional with JDK Optional.
3. Using `ConcurrentMap` as a cache when Guava cache APIs are available.
4. Unbounded Guava caches causing memory pressure.
5. Returning JDK immutable collections from public APIs and triggering null-
   hostile behavior (for example `contains(null)` throwing).
6. Excessive immutable copy creation in hot paths.
7. Utility sprawl reducing readability.

## Do / Don't Examples
### 1. Optional Type
```text
Don't: com.google.common.base.Optional in new code.
Do:    java.util.Optional for modern Java APIs.
```

### 2. Cache Policy
```text
Don't: use ConcurrentMap as a cache or cache without size/expiry constraints.
Do:    use Guava Cache/LoadingCache with explicit maximumSize and expiration
       policies.
```

### 3. Public API Immutable Collections
```text
Don't: return JDK immutable collections from public APIs when Guava is
       available.
Do:    return Guava Immutable* collections (or interfaces backed by them) to
       avoid null-hostile behaviors such as contains(null) throwing.
```

### 4. Library Choice
```text
Don't: add Guava only for a simple string join utility.
Do:    use JDK `String.join` when sufficient.
```

## Code Review Checklist for Guava
- Is Guava usage justified over JDK alternatives?
- If Guava is available, are cache use cases implemented with Guava Cache APIs
  instead of `ConcurrentMap`?
- Are public API collection returns using Guava immutable collections instead of
  JDK immutable collection factories?
- Are immutable collections used where mutability must be controlled?
- Are caches bounded and policy-driven?
- Is Optional usage modern and consistent?
- Does Guava usage improve clarity rather than add dependency noise?

## Testing Guidance
- Test cache policy behavior (eviction/expiration) where used.
- Test immutability assumptions in exposed collection APIs.
- Test public API collection behavior for null probes (for example
  `contains(null)`) where relevant.
- Add regression tests around utility behavior with edge-case inputs.

## Override Notes
- Project-specific utility standards may restrict Guava usage further.
- Baseline rule: prefer standard library first, Guava second with clear value.
- Explicit specialization in this doc: when Guava is available, prefer Guava
  Cache over `ConcurrentMap` for caching and prefer Guava immutable collections
  over JDK immutable collections for public API returns.
