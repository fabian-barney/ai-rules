# Release Management Quick Reference

## Purpose

This quick reference provides the essential steps to prepare a release using the
comprehensive guidance in the RELEASE directory.

## Typical Release Workflow

### 1. Pre-Release Preparation (1-2 weeks before release)

```bash
# Determine next version
CURRENT_VERSION=$(git describe --tags --abbrev=0)
# Decide: MAJOR, MINOR, or PATCH based on changes
NEXT_VERSION="v1.2.0"

echo "Releasing $NEXT_VERSION (from $CURRENT_VERSION)"
```

**Review:** [VERSION_MANAGEMENT.md](VERSION_MANAGEMENT.md) for semantic versioning rules

### 2. Generate Changelog

```bash
# Get changes since last release
git log $CURRENT_VERSION..HEAD --oneline --no-merges

# Get file changes
git diff $CURRENT_VERSION..HEAD --stat
```

**Action:** Create changelog entry following [CHANGELOG_RULES.md](CHANGELOG_RULES.md)

**Output:** Update `CHANGELOG.md` with categorized changes

### 3. Run Security Assessment

```bash
# Create security report directory
mkdir -p security-reports/$NEXT_VERSION

# Run OWASP Dependency-Check
dependency-check \
  --project "MyProject" \
  --scan . \
  --format HTML \
  --format JSON \
  --out security-reports/$NEXT_VERSION/

# Alternative: npm audit
npm audit --json > security-reports/$NEXT_VERSION/npm-audit.json

# Alternative: pip-audit  
pip-audit --format json > security-reports/$NEXT_VERSION/pip-audit.json
```

**Review:**
- [SECURITY_ASSESSMENT.md](SECURITY_ASSESSMENT.md) - Complete framework
- [SECURITY_REPORT_TEMPLATE.md](SECURITY_REPORT_TEMPLATE.md) - Report template
- [SECURITY_REPORTS_STRUCTURE.md](SECURITY_REPORTS_STRUCTURE.md) - Directory structure

**Action:**
1. Review scan results
2. Perform AI contextual analysis for each CVE
3. Complete security-report.md
4. Create cve-analysis.json with risk scores

### 4. Work Through Checklist

**Review:** [RELEASE_CHECKLIST.md](RELEASE_CHECKLIST.md)

Key items:
- [ ] All tests pass
- [ ] Linters pass
- [ ] Documentation updated
- [ ] Security assessment complete
- [ ] Version numbers updated
- [ ] Release notes ready

### 5. Tag and Release

```bash
# Update version in project files
# (package.json, pom.xml, setup.py, etc.)

# Commit changes
git add .
git commit -m "chore: prepare release $NEXT_VERSION"

# Create annotated tag
git tag -a $NEXT_VERSION -m "Release version $NEXT_VERSION"

# Push
git push origin main
git push origin $NEXT_VERSION

# Update security reports latest link
cd security-reports
rm -f latest
ln -s $NEXT_VERSION latest
cd ..
git add security-reports/latest
git commit -m "chore: update security reports latest link"
git push
```

### 6. Publish Artifacts

```bash
# Examples based on your ecosystem
npm publish
# or
mvn deploy
# or
twine upload dist/*
# or
docker push myimage:$NEXT_VERSION
```

### 7. Post-Release

- [ ] Verify published artifacts
- [ ] Announce release
- [ ] Monitor metrics
- [ ] Document lessons learned

**Review:** [RELEASE_BEST_PRACTICES.md](RELEASE_BEST_PRACTICES.md) - Post-release section

## AI Assistant Prompt Examples

### Generate Changelog

```text
Generate a changelog entry for version v1.2.0 based on git history since v1.1.0.

Follow the rules in CHANGELOG_RULES.md:
1. Categorize changes: Added, Changed, Fixed, Security
2. Use imperative mood
3. Focus on user-facing changes
4. Be specific and concise

Here's the commit history:
[paste git log output]
```

### Analyze CVE Contextually

```text
Analyze CVE-2024-12345 in the context of our codebase:

CVE Details:
- CVSS Score: 7.5 (High)
- Affected: vulnerable-lib@1.2.3
- Type: SQL Injection (CWE-89)

Our Usage:
- We use vulnerable-lib for [describe usage]
- It's used in [which components]
- Exposed to [authentication level]

Provide:
1. Exploitability assessment (Not Exploitable, Low, Medium, High, Critical)
2. Impact assessment (Negligible, Low, Medium, High, Critical)
3. Exposure level (Internal, Protected, Exposed, Public)
4. Mitigation status (None, Partial, Complete)
5. Contextual risk score (0-10)
6. Detailed analysis and recommendation

Format the output as JSON following the schema in SECURITY_ASSESSMENT.md.
```

### Compare Security Posture

```text
Compare the security posture between v1.1.0 and v1.2.0:

Previous release (v1.1.0):
- Total CVEs: 8
- Critical: 1, High: 3, Medium: 4

Current release (v1.2.0):
- Total CVEs: 5
- Critical: 0, High: 2, Medium: 3

Identify:
1. Fixed CVEs (in v1.1.0, not in v1.2.0)
2. New CVEs (not in v1.1.0, but in v1.2.0)
3. Persistent CVEs (in both versions)

Generate the comparison section for the security report.
```

## Common Scenarios

### Hotfix Release

For critical bug fixes:

1. **Fast-track the process:**
   - Skip some non-critical checks
   - Focus on testing the specific fix
   - Increment PATCH version

2. **Minimal changelog:**
   - Focus on the critical fix
   - Reference the issue/CVE

3. **Expedited review:**
   - Shorter review period
   - Higher priority deployment

**See:** [RELEASE_CHECKLIST.md](RELEASE_CHECKLIST.md) - Emergency Hotfix section

### Pre-release (Beta/RC)

For testing before stable release:

1. **Version format:** `v1.2.0-beta.1` or `v1.2.0-rc.1`

2. **Mark as pre-release:**
   - In package registry
   - In release notes
   - In documentation

3. **Gather feedback:**
   - Set up feedback channels
   - Document known issues
   - Plan timeline to stable

**See:** [VERSION_MANAGEMENT.md](VERSION_MANAGEMENT.md) - Pre-release Versions

### Major Version (Breaking Changes)

For releases with breaking changes:

1. **Highlight breaking changes:**
   - List prominently in changelog
   - Create migration guide
   - Provide upgrade path

2. **Extended testing:**
   - Longer beta period
   - More extensive QA
   - Community testing

3. **Communication:**
   - Announce well in advance
   - Explain rationale
   - Provide support

**See:** [RELEASE_CHECKLIST.md](RELEASE_CHECKLIST.md) - Major Version section

## Automation Templates

### GitHub Actions Release Workflow

```yaml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Extract version
        id: version
        run: echo "VERSION=${GITHUB_REF#refs/tags/}" >> $GITHUB_OUTPUT
      
      - name: Run tests
        run: npm test
      
      - name: Security scan
        run: |
          mkdir -p security-reports/${{ steps.version.outputs.VERSION }}
          npm audit --json > security-reports/${{ steps.version.outputs.VERSION }}/npm-audit.json
      
      - name: Build
        run: npm run build
      
      - name: Publish
        run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
      
      - name: Create GitHub Release
        uses: actions/create-release@v1
        with:
          tag_name: ${{ steps.version.outputs.VERSION }}
          release_name: Release ${{ steps.version.outputs.VERSION }}
          body_path: ./release-notes.md
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Release Script Template

```bash
#!/bin/bash
# scripts/release.sh - Release automation script

set -e

VERSION=$1
if [ -z "$VERSION" ]; then
  echo "Usage: $0 vX.Y.Z"
  exit 1
fi

echo "🚀 Starting release $VERSION"

# 1. Validate
echo "✓ Validating..."
git diff-index --quiet HEAD || { echo "❌ Uncommitted changes"; exit 1; }

# 2. Test
echo "🧪 Running tests..."
npm test

# 3. Lint
echo "🔍 Linting..."
npm run lint

# 4. Security
echo "🔒 Security scan..."
mkdir -p security-reports/$VERSION
npm audit --json > security-reports/$VERSION/npm-audit.json || true

# 5. Build
echo "🏗️  Building..."
npm run build

# 6. Update version
echo "📝 Updating version..."
npm version $VERSION --no-git-tag-version

# 7. Changelog
echo "📄 Reminder: Update CHANGELOG.md"
read -p "Press enter when changelog is ready..."

# 8. Commit and tag
echo "💾 Committing..."
git add .
git commit -m "chore: release $VERSION"
git tag -a $VERSION -m "Release $VERSION"

# 9. Push
echo "⬆️  Pushing..."
git push origin main
git push origin $VERSION

# 10. Publish
echo "📢 Publishing..."
npm publish

echo "✅ Release $VERSION completed!"
echo ""
echo "Next steps:"
echo "1. Create GitHub release with release notes"
echo "2. Announce the release"
echo "3. Monitor for issues"
```

## Best Practices Summary

1. **Plan Ahead:** Schedule releases, don't rush
2. **Test Thoroughly:** Multiple levels of testing
3. **Document Everything:** Changelog, security reports, release notes
4. **Automate:** CI/CD for consistency
5. **Communicate:** Keep stakeholders informed
6. **Monitor:** Watch metrics after release
7. **Learn:** Retrospectives after each release

## Getting Help

- **General Process:** [RELEASE_CHECKLIST.md](RELEASE_CHECKLIST.md)
- **Version Numbers:** [VERSION_MANAGEMENT.md](VERSION_MANAGEMENT.md)
- **Changelog:** [CHANGELOG_RULES.md](CHANGELOG_RULES.md)
- **Security:** [SECURITY_ASSESSMENT.md](SECURITY_ASSESSMENT.md)
- **Best Practices:** [RELEASE_BEST_PRACTICES.md](RELEASE_BEST_PRACTICES.md)

## Troubleshooting

### Issue: CVE scan shows false positive

**Solution:**
1. Document in security report as accepted risk
2. Provide justification
3. Add to accepted risks section
4. Set review date

**See:** [SECURITY_REPORT_TEMPLATE.md](SECURITY_REPORT_TEMPLATE.md) - Accepted Risks

### Issue: Tests fail before release

**Solution:**
1. Don't release - fix the tests first
2. If not related to your changes, assess risk
3. Document known issues
4. Consider hotfix after release if needed

### Issue: Need to rollback

**Solution:**
1. Follow rollback procedure
2. Communicate the rollback
3. Document the incident
4. Plan the fix
5. Schedule new release

**See:** [RELEASE_BEST_PRACTICES.md](RELEASE_BEST_PRACTICES.md) - Rollback and Recovery

## Summary

This release management framework provides:

- ✅ Structured process for preparing releases
- ✅ Security assessment with AI-powered CVE analysis
- ✅ Comprehensive checklists
- ✅ Version management guidance
- ✅ Automation-ready workflows
- ✅ Best practices for all scenarios

Start with the [RELEASE_CHECKLIST.md](RELEASE_CHECKLIST.md) and refer to other
documents as needed for detailed guidance.
