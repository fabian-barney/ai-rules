# Release Checklist

## Purpose

Provide a comprehensive checklist for preparing and executing a release to
ensure quality, security, and proper documentation.

## Pre-Release Phase

### 1. Version Planning

- [ ] Determine next version number using semantic versioning
  - **Major** (X.0.0): Breaking changes or major new features
  - **Minor** (x.Y.0): New features, backward compatible
  - **Patch** (x.y.Z): Bug fixes, backward compatible
- [ ] Create release branch if using GitFlow: `release/vX.Y.Z`
- [ ] Update version in project files (package.json, pom.xml, setup.py, etc.)

### 2. Code Quality

- [ ] Run full test suite and ensure all tests pass
  ```bash
  # Examples based on project type
  npm test
  mvn test
  pytest
  go test ./...
  ```
- [ ] Run linters and fix any issues
  ```bash
  npm run lint
  mvn checkstyle:check
  flake8
  ```
- [ ] Run static analysis tools
  ```bash
  # Examples
  eslint
  sonarqube
  spotbugs
  ```
- [ ] Check code coverage and ensure it meets thresholds
- [ ] Review and address any TODO or FIXME comments related to this release

### 3. Security Assessment

- [ ] Run dependency vulnerability scan
  ```bash
  # Use appropriate tool for your stack
  dependency-check --project "MyProject" --scan .
  npm audit
  pip-audit
  snyk test
  ```
- [ ] Generate security assessment report (see [SECURITY_ASSESSMENT.md](SECURITY_ASSESSMENT.md))
- [ ] Review all identified CVEs
- [ ] Perform AI-powered contextual analysis for each CVE
- [ ] Document accepted risks
- [ ] Update dependencies with security patches when possible
- [ ] Store security report in `security-reports/vX.Y.Z/`

### 4. Changelog Generation

- [ ] Generate changelog entry for this release (see [CHANGELOG_RULES.md](CHANGELOG_RULES.md))
  ```bash
  # Find last tag
  LAST_TAG=$(git describe --tags --abbrev=0)
  
  # Review changes
  git log $LAST_TAG..HEAD --oneline --no-merges
  git diff $LAST_TAG..HEAD --stat
  ```
- [ ] Categorize changes: Added, Changed, Deprecated, Removed, Fixed, Security
- [ ] Use clear, user-facing language
- [ ] Link to relevant issues and PRs
- [ ] Highlight breaking changes prominently
- [ ] Update CHANGELOG.md with new entry

### 5. Documentation Updates

- [ ] Update README.md with new features or changes
- [ ] Update API documentation if APIs changed
- [ ] Update user guides and tutorials
- [ ] Update migration guides for breaking changes
- [ ] Review and update installation instructions
- [ ] Ensure all documentation links are valid
- [ ] Update screenshots if UI changed

### 6. Build and Artifacts

- [ ] Build release artifacts
  ```bash
  # Examples
  npm run build
  mvn clean package
  docker build -t myapp:vX.Y.Z .
  ```
- [ ] Verify build artifacts are correct and complete
- [ ] Test installation from built artifacts
- [ ] Generate release notes from changelog
- [ ] Prepare distribution packages (npm, Docker, binaries, etc.)

### 7. Testing

- [ ] Run smoke tests on release build
- [ ] Perform manual testing of critical features
- [ ] Test upgrade path from previous version
- [ ] Test rollback procedure
- [ ] Verify backward compatibility (if applicable)
- [ ] Test on all supported platforms/environments
- [ ] Load/performance testing (if applicable)

### 8. Dependencies

- [ ] Review and update all dependencies to latest stable versions (if appropriate)
- [ ] Verify license compatibility for all dependencies
- [ ] Document any dependency changes in changelog
- [ ] Check for deprecated dependencies

## Release Execution

### 9. Version Control

- [ ] Ensure all changes are committed
  ```bash
  git status
  git add .
  git commit -m "Prepare release vX.Y.Z"
  ```
- [ ] Merge release branch to main/master (if using GitFlow)
- [ ] Create and push git tag
  ```bash
  git tag -a vX.Y.Z -m "Release version X.Y.Z"
  git push origin vX.Y.Z
  git push origin main
  ```
- [ ] Merge release branch to develop (if using GitFlow)

### 10. Build and Publish

- [ ] Trigger release build (if automated)
- [ ] Publish to package registries
  ```bash
  # Examples
  npm publish
  mvn deploy
  twine upload dist/*
  docker push myapp:vX.Y.Z
  ```
- [ ] Create GitHub/GitLab release
  - Attach release notes
  - Attach binary artifacts (if applicable)
  - Mark as pre-release if appropriate
- [ ] Update "latest" tags/references

### 11. Documentation Deployment

- [ ] Deploy updated documentation to docs site
- [ ] Update version selector in documentation
- [ ] Ensure old documentation versions remain accessible

### 12. Notifications

- [ ] Post release announcement
  - Changelog highlights
  - Upgrade instructions
  - Breaking changes (if any)
- [ ] Notify stakeholders
  - Development team
  - QA team
  - Product management
  - Customer support
  - End users (via blog, newsletter, etc.)
- [ ] Update project status page/dashboard
- [ ] Close milestone in issue tracker

## Post-Release Phase

### 13. Verification

- [ ] Verify published artifacts are accessible
  ```bash
  # Examples
  npm info mypackage@X.Y.Z
  docker pull myapp:vX.Y.Z
  ```
- [ ] Test installation from published sources
- [ ] Monitor error tracking/logging for issues
- [ ] Review initial user feedback

### 14. Monitoring

- [ ] Monitor deployment metrics
  - Download/installation counts
  - Error rates
  - Performance metrics
  - User adoption
- [ ] Set up alerts for critical issues
- [ ] Monitor security advisories for new CVEs

### 15. Cleanup

- [ ] Delete release branch (if temporary)
- [ ] Archive old artifacts (if applicable)
- [ ] Update project board/backlog
- [ ] Document lessons learned
- [ ] Schedule retrospective (for major releases)

### 16. Next Steps

- [ ] Plan next release
- [ ] Create milestone for next version
- [ ] Prioritize backlog for next iteration
- [ ] Address any deferred issues from this release

## Emergency Hotfix Process

If a critical issue is discovered post-release:

1. **Assess Severity**
   - [ ] Determine if hotfix is necessary
   - [ ] Document the issue and impact

2. **Create Hotfix**
   - [ ] Create hotfix branch from release tag
   - [ ] Implement minimal fix
   - [ ] Test thoroughly
   - [ ] Update changelog with hotfix entry

3. **Release Hotfix**
   - [ ] Increment patch version
   - [ ] Follow abbreviated checklist (focus on testing and security)
   - [ ] Fast-track review process
   - [ ] Release and communicate urgently

4. **Backport**
   - [ ] Merge hotfix to main branch
   - [ ] Merge hotfix to develop branch

## Version-Specific Considerations

### First Release (v1.0.0)

- [ ] Ensure API stability
- [ ] Complete all documentation
- [ ] Establish support channels
- [ ] Set expectations for stability

### Pre-release (alpha/beta/rc)

- [ ] Mark as pre-release in package registry
- [ ] Clearly label as unstable
- [ ] Document known issues
- [ ] Provide feedback channels

### Major Version (vX.0.0)

- [ ] Highlight breaking changes prominently
- [ ] Provide comprehensive migration guide
- [ ] Consider deprecation period for breaking changes
- [ ] Extended testing period
- [ ] Extra communication to users

### Long-Term Support (LTS)

- [ ] Define support timeline
- [ ] Document backport policy
- [ ] Set up maintenance branch
- [ ] Communicate support commitment

## Tools and Automation

### Recommended Tools

- **Version Management**: `standard-version`, `semantic-release`
- **Changelog Generation**: `conventional-changelog`, `git-cliff`
- **Release Automation**: GitHub Actions, GitLab CI, Jenkins
- **Artifact Publishing**: npm, Maven Central, PyPI, Docker Hub
- **Security Scanning**: See [SECURITY_ASSESSMENT.md](SECURITY_ASSESSMENT.md)

### Sample Release Script

```bash
#!/bin/bash
# release.sh - Automated release script

set -e

VERSION=$1
if [ -z "$VERSION" ]; then
  echo "Usage: ./release.sh vX.Y.Z"
  exit 1
fi

echo "🚀 Starting release $VERSION"

# 1. Run tests
echo "📝 Running tests..."
npm test

# 2. Run linter
echo "🔍 Running linter..."
npm run lint

# 3. Security scan
echo "🔒 Running security scan..."
npm audit

# 4. Build
echo "🏗️  Building..."
npm run build

# 5. Update version
echo "📦 Updating version..."
npm version $VERSION --no-git-tag-version

# 6. Generate changelog
echo "📄 Generating changelog..."
# (Use changelog generation tool)

# 7. Commit changes
echo "💾 Committing changes..."
git add .
git commit -m "chore: release $VERSION"

# 8. Tag release
echo "🏷️  Tagging release..."
git tag -a $VERSION -m "Release $VERSION"

# 9. Push
echo "⬆️  Pushing to remote..."
git push origin main
git push origin $VERSION

# 10. Publish
echo "📢 Publishing..."
npm publish

echo "✅ Release $VERSION completed successfully!"
```

## Release Approval

For production releases, obtain approval from:

- [ ] Tech Lead / Engineering Manager
- [ ] Product Manager (for feature releases)
- [ ] Security Team (if security changes)
- [ ] QA Lead (test results verified)

## Sign-off

- **Release Manager:** ___________________
- **Date:** ___________________
- **Version:** ___________________
- **Status:** APPROVED / REJECTED / NEEDS REVISION
