# LOMBOK

Guidance for AI agents implementing and reviewing Lombok usage.

## Scope
- Define when Lombok improves clarity and when explicit code is safer.
- Apply this file to Java classes using Lombok annotations.

## Semantic Dependencies
- Inherit Java baseline from `LANGUAGE/JAVA/JAVA.md` and
  `LANGUAGE/JAVA/EFFECTIVE_JAVA.md`.
- Inherit design/readability constraints from `DESIGN/CLEAN_CODE.md`.

## Defaults
- Use Lombok to remove low-value boilerplate while preserving clarity.
- If Lombok is available, implement simple constructors with Lombok annotations
  (`@NoArgsConstructor`, `@AllArgsConstructor`, `@RequiredArgsConstructor`).
- For dependency injection, prefer `@RequiredArgsConstructor` as the default
  constructor strategy.
- If Lombok is available, implement ordinary getters/setters with
  `@Getter`/`@Setter`.
- If Lombok is available, provide logger instances through the appropriate
  Lombok log annotation (for example `@Slf4j`).
- Prefer `@Builder` for complex immutable object construction.
- Prefer explicit annotations over broad convenience annotations when behavior
  matters.

## Annotation Selection Policy
- `@Getter`/`@Setter`: required for ordinary accessor boilerplate; write manual
  methods only when logic/validation is needed.
- Constructor annotations: use Lombok constructor annotations for simple
  constructor shapes instead of handwritten constructor boilerplate.
- For dependency injection use cases, prefer `@RequiredArgsConstructor`; use
  `@NoArgsConstructor`/`@AllArgsConstructor` only when framework or boundary
  requirements demand those shapes.
- Logging annotations (`@Slf4j`, `@Log4j2`, etc.): required for logger fields
  instead of manual logger instance declarations.
- `@Value` (`lombok.Value`): prefer for immutable DTO/value objects.
- `@Data`: avoid by default on domain entities and types with nuanced identity.
- `@EqualsAndHashCode`: configure intentionally for inheritance/identity.
- `@ToString`: avoid exposing large graphs or sensitive fields.

## Null Handling
- JSpecify nullness annotations take precedence whenever JSpecify is available.
- Lombok/Jakarta nullness annotations are fallback only:
  use `@lombok.NonNull` and `@jakarta.annotation.Nullable` only when JSpecify
  is not available.
- Keep null contracts explicit on fields, parameters, and return types under
  the active nullness annotation strategy.
- Do not choose Lombok nullness annotations when JSpecify is present.

## Risky Annotations and Guardrails
- `@SneakyThrows`: use only with explicit rationale and bounded scope.
- `@Builder.Default`: verify semantics for null/optional behavior.
- `@SuperBuilder`: use cautiously; avoid deep inheritance complexity.
- Keep generated behavior understandable to reviewers.

## Tooling and Build Consistency
- Ensure annotation processing is enabled in build and IDE.
- Keep `lombok.config` committed and consistent.
- Configure `lombok.copyableAnnotations` for framework-required constructor
  annotations where needed.
- Avoid IDE/build mismatch where generated code differs.

## High-Risk Pitfalls
1. `@Data` on entities with incorrect equals/hashCode semantics.
2. Hidden behavior from broad annotations reducing readability.
3. `@ToString` leaking sensitive or massive object graphs.
4. `@SneakyThrows` masking API exception contracts.
5. Annotation processing misconfiguration causing compile/runtime drift.
6. Handwritten no-arg/all-arg/required-arg constructors despite Lombok
   availability.
7. Handwritten ordinary getters/setters despite Lombok availability.
8. Manual logger field declarations instead of Lombok logging annotations.
9. Implicit nullability contracts or use of Lombok/Jakarta nullness annotations
   when JSpecify is available.

## Do / Don't Examples
### 1. Entity Identity
```text
Don't: @Data on JPA entity with mutable identity semantics.
Do:    selective Lombok annotations + deliberate equals/hashCode strategy.
```

### 2. Exception Transparency
```text
Don't: use @SneakyThrows to bypass checked exception design.
Do:    model exception flow explicitly unless justified otherwise.
```

### 3. Constructor Injection
```text
Don't: field injection with mutable dependencies.
Do:    @RequiredArgsConstructor + final dependencies.
```

### 4. Ordinary Accessors
```text
Don't: hand-write trivial getters/setters when Lombok is available.
Do:    use @Getter/@Setter and keep custom methods only for business logic.
```

### 5. Logger Wiring
```text
Don't: declare manual static logger fields in Lombok-enabled classes.
Do:    use appropriate Lombok log annotation (for example @Slf4j).
```

### 6. Nullness Strategy Precedence
```text
Don't: use Lombok/Jakarta nullness annotations when JSpecify is available.
Do:    use JSpecify when available; otherwise use @lombok.NonNull and
       @jakarta.annotation.Nullable as fallback.
```

## Code Review Checklist for Lombok
- Is Lombok reducing boilerplate without hiding critical behavior?
- Are simple constructors (`@NoArgsConstructor`, `@AllArgsConstructor`,
  `@RequiredArgsConstructor`) Lombok-generated when Lombok is available?
- For DI classes, is `@RequiredArgsConstructor` used by default?
- Are ordinary getters/setters Lombok-generated when Lombok is available?
- Are logger instances provided via Lombok log annotations (for example
  `@Slf4j`)?
- Is nullness annotation precedence correct (JSpecify first; Lombok/Jakarta
  fallback only when JSpecify is unavailable)?
- Under the chosen strategy, are fields/parameters/return types explicitly
  annotated for null contracts?
- Are risky annotations (`@Data`, `@SneakyThrows`) justified?
- Are equality/toString semantics safe and intentional?
- Is sensitive data excluded from generated toString output?
- Is lombok config/tooling consistency ensured?
- Are generated constructors aligned with DI framework needs?

## Testing Guidance
- Test equality/hashCode behavior for classes using generated methods.
- Test serialization/mapping behavior for Lombok-built DTOs.
- Validate build + IDE annotation processing consistency in CI.
- Add null-contract tests matching the active strategy (JSpecify if available;
  otherwise Lombok/Jakarta fallback annotations).
- Add regression tests when Lombok annotation strategy changes.

## Override Notes
- If project policy prefers explicit boilerplate in critical modules, follow
  stricter module policy. Baseline rule: favor clarity over annotation density.
- Explicit specialization in this doc: when Lombok is available, constructor,
  ordinary accessor, and logger boilerplate should be Lombok-generated.
- Nullness specialization: JSpecify always takes precedence; Lombok/Jakarta
  nullness annotations are fallback only when JSpecify is unavailable.
