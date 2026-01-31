# REVIEW

## Focus Areas (ordered)
- Correctness and regression risk
- Security and privacy
- Data integrity and error handling
- Observability and logging
- Performance and scalability
- Test coverage and verification gaps
- Cognitive complexity and readability (identify complex methods and propose
  refactors to reduce branching and nesting)

## Dependency Checks
- Newly added public dependencies must align with the selection guidance in
  `AI/FRAMEWORK/FRAMEWORK.md` (and `AI/LIBRARY/LIBRARY.md` where applicable).
- Call out any dependency that appears to be niche, unmaintained, or a poor fit
  for enterprise requirements.

## Output Expectations
- List concrete findings with file references.
- State what was not verified.
- Suggest targeted tests when relevant.
- When cognitive complexity is high, suggest concrete refactors (e.g., extract
  methods, guard clauses, early exits, split responsibilities, introduce strategy).

## Repository Review (2026-01-31)
- Scope: documentation-only repository; no automated builds or tests are defined.
- Strengths: clear entry point (`AI/AI.md`) and consistent domain-structured folders
  (CORE, REVIEW, SECURITY, TEST, LANGUAGE, DESIGN, ARCHITECTURE, FRAMEWORK,
  LIBRARY, COMPLIANCE).
- Gaps:
  - No documented process for updating rules (e.g., CONTRIBUTING, review/approval
    workflow, versioning policy beyond tags).
  - No changelog or release notes to accompany tagged versions.
  - No automation (CI) for link checking or markdown linting to prevent drift.
- Not verified: builds, tests, or external links (none defined).
- Recommended next steps: add a lightweight CONTRIBUTING guide, changelog per
  release, and a basic CI job for markdown/link validation to keep guidance
  trustworthy.
