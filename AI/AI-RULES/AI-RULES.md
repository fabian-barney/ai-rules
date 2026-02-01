# AI-RULES

Guidance for maintaining this ai-rules repository.

## Terminology
- "ai-rules" refers to this repository's baseline ruleset and its published tags.

## Structure
- Keep `AI/AI.md` as the single entry point and link only one level deeper.
- Add new categories as top-level folders under `AI/` and index them in `AI/AI.md`.
- Keep category index files limited to one level down (no cross-links between peers).
- Name index files after their parent directory (e.g., `AI/CORE/CORE.md`, `AI/BUILD_TOOLS/BUILD_TOOLS.md`).
- Use "## Files" as the link section heading in all index files.
- Link AI-RULES guidance files here.

## Files
- [UPDATE.md](UPDATE.md)
- [LESSONS_LEARNED/LESSONS_LEARNED.md](LESSONS_LEARNED/LESSONS_LEARNED.md)

## Maintenance
- Update `README.md` structure list when adding or renaming top-level folders.
- Avoid self-referential links in index files.
- Ensure index files have exactly one "## Files" section (no duplicates).
- Keep markdown lint-friendly formatting (single trailing newline, <= 120 chars per line).
- Use UTF-8 without BOM for Markdown files.
- If a task requires a second attempt, add a short lesson under `AI/AI-RULES/LESSONS_LEARNED/` and link it.
- For new or moved files, update all affected indexes in the same change (AI/AI.md, category index,
  and README structure list).
