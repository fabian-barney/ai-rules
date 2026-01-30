# AI Rules (Baseline)

This repository contains shared, versioned AI guidance intended to be vendored
into other projects (e.g., via git subtree). It is the single source of truth
for baseline agent rules across repositories.

## Entry Point
- `AI/INDEX.md`

## Setup Template
- `AGENTS_TEMPLATE.md` - copy into a project root **as `AGENTS.md`**, then ask
  your AI agent to set up ai-rules as a subtree.

## Structure
- `AI/CORE/`
- `AI/REVIEW/`
- `AI/SECURITY/`
- `AI/TEST/`
- `AI/LANGUAGE/`
- `AI/DESIGN/`
- `AI/ARCHITECTURE/`
- `AI/FRAMEWORK/`
- `AI/LIBRARY/`
- `AI/COMPLIANCE/`

## Usage (git subtree)
Example:
```bash
git subtree add --prefix docs/ai https://github.com/fabian-barney/ai-rules.git main --squash
```

Update later:
```bash
git subtree pull --prefix docs/ai https://github.com/fabian-barney/ai-rules.git main --squash
```

## Versioning
Tag releases (e.g., `v1.0.0`) and pin subtree updates to a tag when stability
is required.
