# DOWNSTREAM-OVERRIDES

Meta guidance for maintainers defining how downstream-project overrides should
be authored by AI agents.

## Scope
- Applies only to this repository.
- Do not copy these rules verbatim into downstream-projects.

## Path Model
- Use placeholders in guidance and templates:
  - `<AI_ROOT_PATH>`
  - `<AI_RULES_PATH>`
  - `<AI_PROJECT_PATH>`
- Default mapping:
  - `<AI_ROOT_PATH>=docs/ai`
  - `<AI_RULES_PATH>=<AI_ROOT_PATH>/AI-RULES`
  - `<AI_PROJECT_PATH>=<AI_ROOT_PATH>/PROJECT`
- Never hardcode `docs/ai` when placeholders are available.

## Override Semantics
- Baseline rules under `<AI_RULES_PATH>` remain authoritative defaults.
- Downstream files under `<AI_PROJECT_PATH>` extend baseline rules.
- Default conflict behavior: downstream override takes precedence.
- Exception: if a baseline rule explicitly forbids override, baseline wins.
- Missing override content always falls back to baseline behavior.

## File Placement Rules
- Override baseline topics at the same relative path under
  `<AI_PROJECT_PATH>` where appropriate.
- Materialize only files and directories that are actually overridden or added.
- Avoid copying untouched baseline files into `<AI_PROJECT_PATH>`.
- Downstream-projects may add additional directories/files not present in
  baseline when they extend project-specific guidance.

## Link and Index Rules
- Downstream override entry point is `<AI_PROJECT_PATH>/AI.md`.
- Every Markdown file under `<AI_PROJECT_PATH>` must be transitively reachable
  from `<AI_PROJECT_PATH>/AI.md`.
- While building link chains, mirror baseline structure where appropriate.
- Additional downstream-only files must also be included in the same reachable
  index chain.
- Maintain index hygiene consistently:
  - Keep clear index files for directories with multiple children.
  - Keep links updated whenever files move or new files are added.

## Authoring Workflow for AI Agents
1. Resolve placeholders from the target downstream-project configuration.
2. Decide whether the new rule is:
   - an override of an existing baseline topic, or
   - a downstream-specific extension.
3. Create/update files under `<AI_PROJECT_PATH>` using placement rules above.
4. Update index and parent-link chains so every touched file is reachable from
   `<AI_PROJECT_PATH>/AI.md`.
5. Verify precedence notes are not contradicting non-overridable baseline
   constraints.
6. Validate links and structure before finalizing changes.

## Backward Compatibility
- If legacy `AI_PROJECT.md` is present, treat it as migration input.
- Prefer converging to `<AI_PROJECT_PATH>/AI.md` as canonical downstream
  override entry point.
- Document migration behavior explicitly in setup/update guidance.
