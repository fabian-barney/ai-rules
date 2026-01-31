# Changelog Generation Rules

## Purpose

Generate clear, consistent CHANGELOG.md entries that document changes between
releases based on git history since the last version tag.

## Format

Follow [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) format:

- Group changes by category: Added, Changed, Deprecated, Removed, Fixed, Security
- Use clear, user-facing language
- Link to relevant issues/PRs when available
- Date format: YYYY-MM-DD

## AI Rules for Changelog Generation

### Discovery Phase

1. **Identify Version Tags**
   ```bash
   git tag -l --sort=-version:refname
   ```
   - Find the most recent version tag (e.g., `v1.0.1`)
   - Determine the next version number based on semantic versioning

2. **Extract Changes Since Last Tag**
   ```bash
   git log <last-tag>..HEAD --oneline
   git log <last-tag>..HEAD --pretty=format:"%h %s" --no-merges
   ```
   - Review all commits since the last release
   - Exclude merge commits unless they provide important context

3. **Analyze Diff**
   ```bash
   git diff <last-tag>..HEAD --stat
   git diff <last-tag>..HEAD -- <specific-file>
   ```
   - Understand the scope of changes
   - Identify modified areas (features, fixes, docs, etc.)

### Categorization Rules

**Added** - New features, capabilities, or files
- New API endpoints
- New configuration options
- New dependencies
- New documentation sections

**Changed** - Modifications to existing functionality
- Updated dependencies
- Refactored code (user-visible impact)
- Modified behavior
- Performance improvements

**Deprecated** - Features marked for future removal
- APIs with recommended alternatives
- Configuration options being phased out

**Removed** - Deleted features or capabilities
- Removed APIs
- Removed dependencies
- Deleted configuration options

**Fixed** - Bug fixes
- Crash fixes
- Logic errors
- Security vulnerabilities (also list in Security)

**Security** - Security-related changes
- CVE fixes
- Security improvements
- Vulnerability patches

### Writing Guidelines

1. **Use Imperative Mood**
   - Good: "Add support for feature X"
   - Bad: "Added support for feature X"

2. **Be Specific and Concise**
   - Good: "Fix race condition in authentication flow"
   - Bad: "Fix bug"

3. **Focus on User Impact**
   - Describe what changed for users, not implementation details
   - Exception: Technical projects may include implementation notes

4. **Group Related Changes**
   - Combine related commits into single changelog entry
   - Example: Multiple commits fixing the same issue → one "Fixed" entry

5. **Prioritize Important Changes**
   - List breaking changes first
   - Highlight security fixes
   - Major features before minor improvements

### Example Changelog Entry

```markdown
## [v1.2.0] - 2026-02-15

### Added
- Support for OAuth2 authentication flow (#123)
- New `--verbose` flag for detailed logging
- API endpoint for bulk data export

### Changed
- Updated dependency `library-x` from 2.0 to 3.0
- Improved performance of data processing by 40%
- Refactored configuration system for better modularity

### Fixed
- Race condition in concurrent request handling (#145)
- Memory leak in long-running background tasks
- Incorrect error messages in validation logic

### Security
- Patched CVE-2026-1234: XSS vulnerability in user input
- Updated `vulnerable-lib` to version 2.3.1
```

## AI Workflow

When asked to generate a changelog entry:

1. **Gather Information**
   ```bash
   # Find last tag
   LAST_TAG=$(git describe --tags --abbrev=0)
   
   # Get commit history
   git log $LAST_TAG..HEAD --oneline --no-merges
   
   # Get file changes
   git diff $LAST_TAG..HEAD --name-status
   ```

2. **Analyze Commits**
   - Parse commit messages
   - Look for conventional commit prefixes (feat:, fix:, docs:, etc.)
   - Identify patterns and themes

3. **Group by Category**
   - Sort changes into appropriate categories
   - Combine related changes
   - Remove noise (typo fixes, formatting changes)

4. **Draft Entry**
   - Write clear, concise descriptions
   - Follow format guidelines
   - Include relevant references

5. **Review and Refine**
   - Check for completeness
   - Ensure user-facing language
   - Verify consistency with existing changelog

## Special Cases

### First Release
- Document all major features
- Provide overview of initial capabilities
- Set expectations for stability

### Breaking Changes
- Highlight prominently at the top
- Provide migration guidance
- Link to detailed migration docs

### Pre-release Versions
- Use version format: `v1.2.0-beta.1`
- Mark as pre-release
- Note known issues or limitations

### Hotfix Releases
- Focus on the specific fix
- Reference the vulnerability or critical bug
- Keep other changes minimal

## Anti-Patterns

❌ **Avoid:**
- "Various fixes" - Too vague
- "Updated code" - No useful information
- "WIP" or "temp commit" - Should be squashed
- Implementation details without user impact
- Duplicate entries across categories
- Inconsistent formatting or style

✅ **Prefer:**
- Specific, actionable descriptions
- User-centric language
- Consistent categorization
- Links to issues/PRs for context
- Clear indication of breaking changes
