# CORE

Core, non-negotiable rules that apply to all projects.

## Purpose
These rules define the baseline expectations for AI-assisted work across
projects. Projects may add an overlay file for local constraints.

## General
- Prefer clarity over cleverness.
- Keep changes small and reviewable.
- Avoid duplicating knowledge across files; keep a single source of truth.
- Do not claim tests or scans ran unless they were actually executed.

## Documentation
- Keep doc filenames consistent and UPPERCASE at the repo root.
- Record non-obvious decisions as Architecture Decision Records (ADR) in
  `docs/decisions/`.
- ADR filenames use `ADR-0001-TITLE.md` (UPPERCASE, hyphenated).

## Architecture Precedence
- Safety/security/correctness rules take priority.
- Framework architectural conventions are the default.
- General architecture guidance (e.g., Clean Architecture) applies when it does
  not conflict with the framework.
- Conflicts should be documented with a brief ADR when material.

## Safety
- Never include secrets or proprietary data in prompts or logs.
- Ask if a request could cause destructive changes.
