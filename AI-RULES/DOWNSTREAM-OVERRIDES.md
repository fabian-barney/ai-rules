# DOWNSTREAM-OVERRIDES

Meta guidance for maintainers defining how downstream-project extensions should
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

## Extension Semantics and Conflict Precedence
- Baseline rules under `<AI_RULES_PATH>` remain authoritative defaults.
- Downstream files under `<AI_PROJECT_PATH>` extend baseline rules.
- Default conflict behavior: downstream extension takes precedence.
- Exception: if a baseline rule explicitly forbids extension/override for that
  rule, baseline wins.
- Missing downstream extension content always falls back to baseline behavior.

## File Placement Rules
- Extend baseline topics at the same relative path under
  `<AI_PROJECT_PATH>` where appropriate.
- Materialize only files and directories that are actually extended or added.
- Avoid copying untouched baseline files into `<AI_PROJECT_PATH>`.
- Downstream-projects may add additional directories/files not present in
  baseline for project-specific guidance not covered by baseline.

## Link and Index Rules
- Downstream extension entry point is `<AI_PROJECT_PATH>/AI.md`.
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
   - an extension of an existing baseline topic, or
   - a new downstream-specific topic or rule not present in baseline.
3. Create/update files under `<AI_PROJECT_PATH>` using placement rules above.
4. Update index and parent-link chains so every touched file is reachable from
   `<AI_PROJECT_PATH>/AI.md`.
5. Verify precedence notes are not contradicting non-overridable baseline
   constraints.
6. Validate links and structure before finalizing changes.
