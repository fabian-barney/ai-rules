# AGENTS.md - AI Rules Setup Template

## Purpose
This file bootstraps the ai-rules baseline into a new project. Copy this file
into your project root as `AGENTS.md` and ask your AI agent to run the setup.

## Setup (run once)
1. Add the ai-rules subtree:
   git subtree add --prefix docs/ai https://github.com/fabian-barney/ai-rules.git main --squash
2. Baseline entry point (after subtree add):
   docs/ai/AI/INDEX.md
3. Create a local overlay for project-specific rules (recommended):
   AI_PROJECT.md
   Note: keep this outside `docs/ai/` so subtree updates do not overwrite it.
4. Update your project `AGENTS.md` to reference:
   - Baseline: docs/ai/AI/INDEX.md
   - Overlay: AI_PROJECT.md

## Updates
To pull new baseline rules later:
  git subtree pull --prefix docs/ai https://github.com/fabian-barney/ai-rules.git main --squash

## Conventions
- Root-level docs use UPPERCASE filenames.
- Avoid case-only renames on Windows.

## Access
This repository is private; ensure your GitHub account has access.
