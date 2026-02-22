# LANGUAGE

Language-layer contract for syntax, semantics, naming, and readability rules.

## Role in the Ruleset
- LANGUAGE sits between cross-cutting baseline docs and framework/library docs.
- Frameworks and libraries inherit language constraints before applying their
  own specialization.
- Global precedence and override semantics are defined in
  `CORE/RULE_DEPENDENCY_TREE.md`.

## Scope Boundary
LANGUAGE includes:
- Language-agnostic coding conventions and readability constraints.
- Language-specific defaults and pitfalls (for example Java, TypeScript).
- Language-level naming/casing and typing rules.

LANGUAGE does not include:
- Framework lifecycle or rendering behavior.
- Library API usage rules.
- Build, deployment, or CI runtime behavior.

Those belong in `FRAMEWORK/**`, `LIBRARY/**`, `BUILD_TOOLS/**`,
`INFRASTRUCTURE/**`, and `CI-CD/**`.

## Inheritance and Specialization Contract
- Apply `CONVENTIONS.md` and `READABILITY.md` to all languages by default.
- Apply the specific language document next (for example Java or TypeScript).
- If a specialized language extends another language model (for example
  TypeScript over JavaScript), the specialization may narrow rules but must not
  silently weaken inherited correctness/safety constraints.
- Framework docs may specialize language usage patterns, but only with explicit
  rationale in that framework doc.

## Files
- [CONVENTIONS.md](CONVENTIONS.md) - General coding conventions.
- [READABILITY.md](READABILITY.md) - Readability and clarity guidance.

## Languages
- [JAVASCRIPT/JAVASCRIPT.md](JAVASCRIPT/JAVASCRIPT.md) - JavaScript runtime
  baseline (parent for TypeScript and JS frameworks).
- [TYPESCRIPT/TYPESCRIPT.md](TYPESCRIPT/TYPESCRIPT.md) - TypeScript
  specialization over JavaScript baseline.
- [JAVA/JAVA.md](JAVA/JAVA.md) - Java language baseline and entry point.
- [JAVA/EFFECTIVE_JAVA.md](JAVA/EFFECTIVE_JAVA.md) - Effective Java
  specialization and design heuristics.
- [PYTHON/PYTHON.md](PYTHON/PYTHON.md) - Python language baseline.
- [GO/GO.md](GO/GO.md) - Go language baseline.
- [C_SHARP/C_SHARP.md](C_SHARP/C_SHARP.md) - C#/.NET language baseline.
- [RUST/RUST.md](RUST/RUST.md) - Rust language baseline.
- [KOTLIN/KOTLIN.md](KOTLIN/KOTLIN.md) - Kotlin language baseline.
- [PHP/PHP.md](PHP/PHP.md) - PHP language baseline.
- [RUBY/RUBY.md](RUBY/RUBY.md) - Ruby language baseline.
- [SWIFT/SWIFT.md](SWIFT/SWIFT.md) - Swift language baseline.

## Languages and Formats
- [HTML/HTML.md](HTML/HTML.md) - Semantic and accessible HTML baseline.
- [CSS/CSS.md](CSS/CSS.md) - Maintainable and accessible CSS baseline.
- [SQL/SQL.md](SQL/SQL.md) - SQL correctness/performance baseline.
- [YAML/YAML.md](YAML/YAML.md) - YAML configuration safety baseline.
- [SHELL/SHELL.md](SHELL/SHELL.md) - Shell scripting safety baseline.

## Authoring Notes
- Keep this file index-level and boundary-focused.
- Put deep language behavior in language-specific docs, not here.
- When adding a new language, add it to this index and align semantic
  dependencies in `CORE/RULE_DEPENDENCY_TREE.md`.
