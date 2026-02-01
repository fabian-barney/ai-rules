# RELEASE

Release process for the ai-rules repository (not downstream projects).

## Scope
- Applies only to this repository.
- Do not copy these rules into consuming projects.

## Steps
1. Ensure `main` is up to date and all release PRs are merged.
2. Update `CHANGELOG.md`:
   - Add a new version section with today's date.
   - Summarize user-visible changes.
3. Update versioned examples to the new release tag (for example, prompts in `README.md`).
4. Commit the changelog and example updates with a release prep message.
5. Create an annotated tag (for example `v2.0.0`).
6. Push `main` and the tag to GitHub.
7. Create a GitHub Release using the changelog notes.
8. Verify the release page and tag exist.

## Notes
- Prefer tagged releases; do not release from feature branches.
- Keep release notes concise and aligned with the changelog.
