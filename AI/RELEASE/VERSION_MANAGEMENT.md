# Version Management

## Purpose

Define best practices and rules for version numbering, tagging, and lifecycle
management to ensure consistent and predictable releases.

## Semantic Versioning

Follow [Semantic Versioning 2.0.0](https://semver.org/) (SemVer):

### Format: MAJOR.MINOR.PATCH

- **MAJOR** (X.0.0): Incompatible API changes or breaking changes
- **MINOR** (x.Y.0): New functionality in a backward-compatible manner
- **PATCH** (x.y.Z): Backward-compatible bug fixes

### Pre-release Versions

Format: `X.Y.Z-prerelease.N`

- **Alpha**: `v1.0.0-alpha.1`, `v1.0.0-alpha.2`, ...
  - Early development, unstable, feature incomplete
  - Internal testing only
  
- **Beta**: `v1.0.0-beta.1`, `v1.0.0-beta.2`, ...
  - Feature complete, but may have bugs
  - Ready for wider testing
  
- **Release Candidate**: `v1.0.0-rc.1`, `v1.0.0-rc.2`, ...
  - Potentially final version unless bugs found
  - Final testing before release

### Build Metadata

Format: `X.Y.Z+metadata`

Example: `v1.0.0+20240115.gitabc1234`

- Build metadata does not affect version precedence
- Useful for CI/CD builds and internal tracking

## Version Decision Matrix

### When to Increment MAJOR

❗ **Breaking Changes** - Increment when:

- Removing or renaming public APIs
- Changing API signatures (parameters, return types)
- Changing behavior in incompatible ways
- Removing support for dependencies or platforms
- Changing data formats (breaking backward compatibility)
- Changing configuration schema (breaking existing configs)

**Exception**: Version 0.x.x is for initial development; breaking changes are
allowed in MINOR versions during this phase.

### When to Increment MINOR

✨ **New Features** - Increment when:

- Adding new public APIs
- Adding new functionality (backward compatible)
- Marking existing features as deprecated (but still functional)
- Adding optional parameters
- Significant internal refactoring (user-visible improvements)
- Performance improvements worth noting

### When to Increment PATCH

🐛 **Bug Fixes** - Increment when:

- Fixing bugs without changing public interface
- Security patches (also document in Security category)
- Documentation fixes (if standalone release)
- Internal refactoring with no user-visible changes
- Dependency updates (no feature/breaking changes)

## Version Lifecycle

### Initial Development (0.x.x)

- **0.1.0**: First release with minimal viable features
- **0.x.0**: Breaking changes allowed in MINOR version
- **0.x.y**: Bug fixes and minor improvements
- **1.0.0**: First stable release

### Stable Releases (1.x.x+)

- Follow SemVer strictly
- Breaking changes require MAJOR bump
- Deprecate before removing in next MAJOR

### Long-Term Support (LTS)

Selected versions receive extended support:

- Security updates backported
- Critical bug fixes backported
- Documented support timeline (e.g., 2 years)
- Branch naming: `support/v1.x`, `support/v2.x`

Example LTS Schedule:
```
v1.0.0 ───→ v2.0.0 ───→ v3.0.0
  │            │            │
  │ (LTS)      │ (Active)   │ (Active)
  │            │            │
  └─ 2yr ──────┴─ 2yr ──────┴→
```

### End of Life (EOL)

- Announce EOL at least 6 months in advance
- Provide migration path to current version
- Document final supported features
- Archive documentation

## Git Tagging Strategy

### Tag Format

Use annotated tags with `v` prefix:

```bash
# Good
git tag -a v1.2.3 -m "Release version 1.2.3"

# Avoid
git tag 1.2.3  # Missing 'v' prefix
git tag v1.2.3 # Lightweight tag (no -a)
```

### Tag Message Format

```
Release version X.Y.Z

Summary of key changes:
- Feature 1
- Feature 2
- Important fix

See CHANGELOG.md for full details.
```

### Tagging Workflow

```bash
# 1. Ensure you're on the right commit
git log --oneline -5

# 2. Create annotated tag
git tag -a vX.Y.Z -m "Release version X.Y.Z"

# 3. Verify tag
git show vX.Y.Z

# 4. Push tag to remote
git push origin vX.Y.Z

# 5. Push all tags (if needed)
git push origin --tags
```

### Tag Management

```bash
# List all tags
git tag -l

# List tags matching pattern
git tag -l "v1.*"

# Delete local tag
git tag -d vX.Y.Z

# Delete remote tag
git push origin --delete vX.Y.Z

# Rename tag (delete and recreate)
git tag vX.Y.Z-new vX.Y.Z
git tag -d vX.Y.Z
git push origin vX.Y.Z-new
git push origin --delete vX.Y.Z
```

## Branch Strategy

### GitFlow

```
main (production-ready)
  ├── develop (integration)
  │   ├── feature/new-feature
  │   └── feature/another-feature
  ├── release/vX.Y.Z (preparing release)
  └── hotfix/vX.Y.Z+1 (emergency fixes)
```

**Workflow:**
1. Feature branches from `develop`
2. Release branches from `develop`
3. Hotfix branches from `main`
4. Tags created on `main` after merging release/hotfix

### Trunk-Based Development

```
main (always releasable)
  ├── short-lived feature branches
  └── tags: v1.0.0, v1.1.0, v1.1.1, ...
```

**Workflow:**
1. Short-lived feature branches
2. Merge to `main` frequently
3. Tag `main` when ready to release
4. Use feature flags for incomplete features

### GitHub Flow

```
main (production)
  ├── feature-branch-1
  └── feature-branch-2
```

**Workflow:**
1. Branch from `main`
2. Open PR for review
3. Deploy from branch for testing
4. Merge to `main` and tag

## Version File Management

### Node.js (package.json)

```json
{
  "name": "my-package",
  "version": "1.2.3",
  "scripts": {
    "version": "npm run build && git add -A dist",
    "postversion": "git push && git push --tags"
  }
}
```

Update version:
```bash
npm version major|minor|patch
# Or specify exact version
npm version 1.2.3
```

### Python (setup.py / pyproject.toml)

```python
# setup.py
setup(
    name='my-package',
    version='1.2.3',
)
```

```toml
# pyproject.toml
[project]
name = "my-package"
version = "1.2.3"
```

### Java (pom.xml)

```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>my-app</artifactId>
    <version>1.2.3</version>
</project>
```

Update version:
```bash
mvn versions:set -DnewVersion=1.2.3
mvn versions:commit
```

### Go (go.mod)

```go
module github.com/user/repo

go 1.21
```

Version is tracked via git tags:
```bash
git tag v1.2.3
git push origin v1.2.3
```

### Docker

```dockerfile
# Dockerfile
FROM node:18-alpine
LABEL version="1.2.3"
```

Tag images:
```bash
docker build -t myapp:1.2.3 .
docker build -t myapp:latest .
docker push myapp:1.2.3
docker push myapp:latest
```

## Version Automation

### standard-version (Node.js)

```bash
npm install --save-dev standard-version

# Add to package.json
{
  "scripts": {
    "release": "standard-version"
  }
}

# Create release
npm run release          # Auto-increment based on commits
npm run release -- --release-as minor  # Force minor
npm run release -- --release-as 1.2.3  # Exact version
```

### semantic-release (Node.js)

Fully automated versioning and publishing:

```json
// package.json
{
  "devDependencies": {
    "semantic-release": "^19.0.0"
  }
}
```

Uses commit message conventions to determine version bump.

### Conventional Commits

Format: `<type>(<scope>): <subject>`

Types:
- `feat:` → MINOR version bump
- `fix:` → PATCH version bump
- `BREAKING CHANGE:` → MAJOR version bump
- `docs:`, `chore:`, `style:`, `refactor:`, `test:` → No version bump

Examples:
```
feat(auth): add OAuth2 support
fix(api): correct response status codes
docs: update installation guide
BREAKING CHANGE: remove legacy API endpoint
```

## Version Communication

### Release Notes

Include for each release:
- Version number
- Release date
- Summary of changes
- Breaking changes (if any)
- Upgrade instructions
- Known issues
- Contributors

### Deprecation Policy

1. **Announce Deprecation**: Document in changelog, add warnings
2. **Grace Period**: At least one MAJOR version cycle
3. **Remove**: In next MAJOR version
4. **Communicate**: Clear migration path

Example:
```
v2.0.0: oldFunction() deprecated, use newFunction()
v2.1.0: oldFunction() still available with deprecation warning
v3.0.0: oldFunction() removed
```

### Version Support Matrix

Document supported versions:

| Version | Status   | Released | EOL Date | Support Level   |
|---------|----------|----------|----------|-----------------|
| 3.1.x   | Active   | 2024-12  | TBD      | Full            |
| 3.0.x   | Active   | 2024-06  | 2025-06  | Full            |
| 2.5.x   | LTS      | 2023-12  | 2025-12  | Security only   |
| 2.4.x   | EOL      | 2023-06  | 2024-06  | None            |

## AI Rules for Version Management

When determining version number:

1. **Analyze Changes**
   ```bash
   git log <last-tag>..HEAD --oneline
   git diff <last-tag>..HEAD
   ```

2. **Classify Changes**
   - Breaking changes → MAJOR
   - New features → MINOR
   - Bug fixes → PATCH

3. **Determine Next Version**
   ```bash
   # Get current version
   CURRENT=$(git describe --tags --abbrev=0)
   
   # Suggest next version based on analysis
   # Output: v1.2.3 → v2.0.0 (breaking changes found)
   ```

4. **Update Version Files**
   - Update package.json, pom.xml, etc.
   - Maintain consistency across all files

5. **Create Tag**
   ```bash
   git tag -a vX.Y.Z -m "Release version X.Y.Z"
   ```

## Best Practices

✅ **Do:**
- Use semantic versioning consistently
- Tag all releases
- Document breaking changes clearly
- Maintain CHANGELOG.md
- Automate version bumping
- Test before releasing
- Communicate deprecations early

❌ **Don't:**
- Skip version numbers
- Use version 1.0.0 prematurely
- Change version without corresponding changes
- Delete published versions
- Reuse version numbers
- Make breaking changes in PATCH/MINOR versions (after 1.0.0)

## Version Comparison

Understanding version precedence:

```
1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-beta < 1.0.0-rc.1 < 1.0.0
1.0.0 < 1.0.1 < 1.1.0 < 2.0.0
1.0.0+build1 = 1.0.0+build2  (metadata ignored)
```

Programmatically:
```bash
# Compare versions
semver compare 1.2.3 1.2.4  # -1
semver compare 1.2.3 1.2.3  #  0
semver compare 1.2.4 1.2.3  #  1
```

## Recovery from Mistakes

### Wrong Version Published

1. **Do not delete**: Published versions should not be removed
2. **Deprecate**: Mark version as deprecated in registry
3. **Publish Corrected**: Release new version (increment PATCH)
4. **Communicate**: Explain the issue and correct version

### Incorrect Tag

```bash
# Fix tag before pushing
git tag -d vX.Y.Z
git tag -a vX.Y.Z -m "Corrected tag"

# If already pushed, coordinate with team
git push origin --delete vX.Y.Z
git tag -a vX.Y.Z -m "Corrected tag"
git push origin vX.Y.Z
```

### Accidental Breaking Change in MINOR/PATCH

1. **Acknowledge**: Document in changelog
2. **Revert**: Consider reverting the change
3. **Release**: Issue new MAJOR version immediately
4. **Communicate**: Apologize and provide migration guide
