# 2026-02-01-release-version-examples

## Scope
- Applies only to this repository.
- Do not copy these rules into downstream-projects.

## Issue
During the v2.1.0 and v2.1.1 releases, versioned example prompts in README.md
were not updated before creating the release commit and tag. This resulted in
the release tag containing outdated version references, requiring a follow-up
commit after the release.

The release instructions in [RELEASE.md](../RELEASE.md) (step 4) clearly stated
to update versioned examples, but the step was skipped.

## Prevention
Before creating the release commit:
1. Search for the last released version tag string (for example, run
   `git grep "v2.1.0"` when releasing `v2.1.1`) across all files.
2. Update every versioned reference found (README example prompts and any other
   documentation) to the new version.
3. Verify the changes are staged before committing.

Checklist for AI agents:
- [ ] Run `git grep "vX.Y.Z"` (using the last released version tag) to find all
      references.
- [ ] Update README.md example prompts and any other versioned documentation.
- [ ] Stage all version updates with the changelog in the same commit.
- [ ] Only then create the annotated tag.
