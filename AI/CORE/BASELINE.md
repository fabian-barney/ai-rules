# BASELINE

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
- Record non-obvious decisions as ADRs.

## Safety
- Never include secrets or proprietary data in prompts or logs.
- Ask if a request could cause destructive changes.
