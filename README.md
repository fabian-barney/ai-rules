# AI Rules (Baseline)

This repository contains shared, versioned AI guidance intended to be vendored
into other projects (e.g., via git subtree). It is the single source of truth
for baseline agent rules across repositories.

## Entry Point
- `AI/AI.md`

## Setup Template
- `AGENTS_TEMPLATE.md` - copy into a project root **as `AGENTS.md`**, then ask
  your AI agent to set up ai-rules. If you need a specific version, say so.
  Example prompt: "setup ai-rules with version v1.0.1"

## Structure
- `AI/CORE/`
- `AI/REVIEW/`
- `AI/SECURITY/`
- `AI/TEST/`
- `AI/LANGUAGE/`
- `AI/DESIGN/`
- `AI/ARCHITECTURE/`
- `AI/FRAMEWORK/`
- `AI/BUILD_TOOLS/`
- `AI/LIBRARY/`
- `AI/COMPLIANCE/`
- `AI/CI-CD/`
- `AI/INFRASTRUCTURE/`

## Contributing
- `CONTRIBUTING.md`

## Changelog
- `CHANGELOG.md`

## Usage (git subtree)
ai-rules can be vendored into another repository (for example with git subtree).
This keeps the rules in sync while still allowing you to pin a specific version.
You do not need to manage the mechanics manually if you use the AI prompts below.

### Update Prompt (for the AI)
Example prompt:
"update ai-rules to version v2.0.0"

If you are not pinning a version, you can ask for the latest tagged release instead.

## Versioning
Tag releases (e.g., `v1.0.0`) and pin subtree updates to a tag when stability
is required.
