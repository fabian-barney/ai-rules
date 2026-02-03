# UPDATE

Instructions for AI agents to update ai-rules in a downstream repository.

## Prompt Examples
- "update ai-rules local"
- "update ai-rules git"
- "update ai-rules local to version v2.0.0"
- "update ai-rules git to version v2.0.0"
- "update ai-rules to the latest tagged release"

## Update Steps (run when requested)
1. Identify the setup mode (local or git) from the user prompt or repository docs.
   If it is not specified, ask which mode to use.
2. Locate the vendored ai-rules path and entry point from `AGENTS.md` or README.
3. Determine the target version:
   - If the user specifies a tag, use it.
   - Otherwise, use the latest tagged release.
4. Update based on mode:
   - If it is git (tracked subtree):
     `git subtree pull --prefix <path> https://github.com/fabian-barney/ai-rules.git <ref> --squash`
     Commit the update and open a PR.
   - If it is local (no commits, no push):
     - Temporarily remove the ai-rules entries from `.git/info/exclude`.
     - If `<path>` already exists locally, remove it only after confirming there is no real work in it.
     - Run:
       `git subtree add --prefix <path> https://github.com/fabian-barney/ai-rules.git <ref> --squash`
       (This creates a commit; if it fails due to missing author identity, set it locally and retry.)
     - If Git requires an author identity, set it locally:
       `git config --local user.name "Your Name"`
       `git config --local user.email "you@example.com"`
     - Undo the commit but keep files:
       `git reset --mixed HEAD~1`
     - Re-add the exclude entries to `.git/info/exclude`.
5. Verify the baseline entry point still resolves (e.g., `<path>/AI/AI.md`).
6. Preserve local overlays and any project-specific rules outside the vendor path.
7. Record the updated version in the destination repository if it tracks versions.
8. Summarize changes. Only open a PR for git mode.

## Expectations
- Prefer tagged releases unless explicitly asked for a branch.
- Do not overwrite local overlays or custom rules.
