# AI-RULES

Guidance for maintaining this ai-rules repository.

## Terminology
- "ai-rules" refers to this repository's baseline ruleset and its published tags.

## Structure
- Keep `AI/AI.md` as the single entry point and link only one level deeper.
- Add new categories as top-level folders under `AI/` and index them in `AI/AI.md`.
- Keep category index files limited to one level down (no cross-links between peers).
- Name index files after their parent directory (e.g., `AI/CORE/CORE.md`, `AI/BUILD_TOOLS/BUILD_TOOLS.md`).
- Link AI-RULES guidance files here.

## AI-RULES Files
- [UPDATE.md](UPDATE.md)

## Maintenance
- Update `README.md` structure list when adding or renaming top-level folders.
- Avoid self-referential links in index files.
- Keep markdown lint-friendly formatting (single trailing newline, <= 120 chars per line).
