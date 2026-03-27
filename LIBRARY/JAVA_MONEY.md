# JAVA_MONEY

Guidance for AI agents implementing and reviewing JavaMoney usage for monetary
values in Java.

## Scope
- Define mandatory modeling and arithmetic rules for monetary values.
- Apply this file whenever Java code stores, computes, compares, or transfers
  money.
- Treat this file as the default monetary approach for Java codebases.

## Semantic Dependencies
- Inherit Java baseline from `LANGUAGE/JAVA/JAVA.md`.
- Inherit design heuristics from `LANGUAGE/JAVA/EFFECTIVE_JAVA.md`.
- Inherit SQL/persistence constraints from `LANGUAGE/SQL/SQL.md`,
  `ARCHITECTURE/N_PLUS_1.md`, and framework docs when persistence is involved.
- Cross-cutting baselines are inherited transitively via the parents above.

## Defaults
- Always prefer JavaMoney (`javax.money`, typically implemented with Moneta)
  for monetary values in Java.
- Model business monetary amounts as `MonetaryAmount` (or domain wrappers
  around it), not `BigDecimal` alone.
- Keep currency explicit for every monetary value.
- Centralize rounding rules with JavaMoney rounding operators.
- Restrict `BigDecimal` usage to integration boundaries where JavaMoney cannot
  be used directly.

## Modeling and API Design
- Use `MonetaryAmount` in domain/service method parameters and return types.
- Do not pass `(BigDecimal amount, String currency)` pairs through internal
  APIs.
- Keep conversion to/from persistence and transport models in boundary adapters.
- Use dedicated domain value objects when additional invariants are required
  (for example non-negative totals).

## Arithmetic, Rounding, and Currency Conversion
- Perform business arithmetic via `MonetaryAmount` operations.
- Define legal rounding behavior once per use case (for example tax, invoice
  total, payout) and reuse it.
- Validate currency compatibility before arithmetic and comparison.
- Use JavaMoney conversion providers for FX conversion; do not hand-roll
  exchange-rate math in core business code.

## High-Risk Pitfalls
1. Representing money as bare `BigDecimal` without currency.
2. Using `double`/`float` for monetary calculations.
3. Applying inconsistent rounding rules across code paths.
4. Mixing currencies in arithmetic without explicit conversion.
5. Spreading persistence/JSON conversion logic through domain services.
6. Using `FastMoney` without explicit precision-tradeoff analysis.

## Do / Don't Examples
### 1. API Modeling
```java
// Don't
public BigDecimal calculateTotal(Order order) { ... }

// Do
public MonetaryAmount calculateTotal(Order order) { ... }
```

### 2. Monetary Value Construction
```java
// Don't
BigDecimal price = new BigDecimal("19.99");
String currency = "USD";

// Do
MonetaryAmount price = Money.of(new BigDecimal("19.99"), "USD");
```

### 3. Rounding Strategy
```text
Don't: call setScale(...) ad-hoc in multiple services.
Do:    centralize and reuse JavaMoney rounding operators per business rule.
```

## Code Review Checklist for JavaMoney
- Are monetary values represented with `MonetaryAmount` (or wrappers)
  end-to-end?
- Is currency always explicit and validated before arithmetic/comparison?
- Are rounding rules centralized and domain-correct?
- Are FX conversions using JavaMoney services instead of manual math?
- Are persistence/transport conversions isolated to boundary layers?
- Are primitive and `BigDecimal` money values confined to explicit interop
  points?

## Testing Guidance
- Test arithmetic and aggregation with same- and mixed-currency scenarios.
- Test rounding rules for edge values (tax, discount, fractional-cent cases).
- Test currency-conversion behavior including missing-rate/error handling.
- Test boundary mapping between `MonetaryAmount` and persistence/transport
  schemas.
- Add regression tests for known monetary defect classes (rounding drift,
  currency mix-ups).

## Override Notes
- This file narrows Java baseline monetary modeling by making JavaMoney the
  default approach.
- If a boundary cannot use JavaMoney, keep conversion localized at that
  boundary and return to JavaMoney types immediately in core logic.
