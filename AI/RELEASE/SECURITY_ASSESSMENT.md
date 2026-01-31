# Security Assessment Framework

## Purpose

Provide a comprehensive security assessment process for each release, including
dependency vulnerability scanning, CVE analysis, and comparison with previous
releases to track security posture improvements.

## Directory Structure

Security reports are stored in a dedicated directory structure:

```
security-reports/
├── v1.0.0/
│   ├── security-report.md
│   ├── dependency-check-report.html
│   ├── dependency-check-report.json
│   └── cve-analysis.json
├── v1.0.1/
│   ├── security-report.md
│   ├── dependency-check-report.html
│   ├── dependency-check-report.json
│   └── cve-analysis.json
└── latest -> v1.0.1/
```

## Security Assessment Process

### 1. Dependency Vulnerability Scanning

Use OWASP Dependency-Check or similar tools:

```bash
# Using OWASP Dependency-Check
dependency-check --project "ProjectName" \
  --scan . \
  --format HTML \
  --format JSON \
  --out security-reports/v1.0.1/

# Using npm audit (Node.js)
npm audit --json > security-reports/v1.0.1/npm-audit.json

# Using pip-audit (Python)
pip-audit --format json > security-reports/v1.0.1/pip-audit.json

# Using Snyk
snyk test --json > security-reports/v1.0.1/snyk-report.json
```

### 2. CVE Analysis

For each identified CVE:

#### A. Extract Public Information

- **CVE ID**: e.g., CVE-2024-12345
- **CVSS Score**: Common Vulnerability Scoring System v3.1
- **Severity Rating**: Critical, High, Medium, Low
- **CWE Classification**: e.g., CWE-79 (XSS), CWE-89 (SQL Injection)
- **Affected Component**: Library name and version range
- **Published Date**: When the CVE was disclosed
- **Description**: Official CVE description
- **References**: Links to advisories, patches, discussions

#### B. AI-Powered Contextual Analysis

Analyze each CVE in the context of the specific codebase:

**Analysis Dimensions:**

1. **Exploitability in This Codebase**
   - Is the vulnerable code path actually used?
   - Are there mitigating factors in the architecture?
   - What attack vectors are available?
   - Rate: Not Exploitable, Low, Medium, High, Critical

2. **Impact Assessment**
   - What data or systems are at risk?
   - What is the blast radius of exploitation?
   - Does it affect core functionality or edge cases?
   - Rate: Negligible, Low, Medium, High, Critical

3. **Exposure Level**
   - Is the vulnerable component exposed to untrusted input?
   - Is it used in public-facing APIs?
   - Is it isolated or deeply integrated?
   - Rate: Internal, Protected, Exposed, Public

4. **Mitigation Status**
   - Are there workarounds in place?
   - Is the vulnerable functionality disabled?
   - Are there security controls that reduce risk?
   - Options: None, Partial, Complete

5. **Contextual Risk Score**
   - Combine above factors into adjusted risk score
   - Formula: `(CVSS * Exploitability * Impact * Exposure) / Mitigation`
   - Result: 0.0 (no risk) to 10.0 (critical)

**AI Analysis Prompt Template:**

```
Analyze CVE-{ID} in the context of this codebase:

CVE Details:
- ID: {CVE_ID}
- CVSS Score: {CVSS_SCORE}
- Affected Library: {LIBRARY}:{VERSION}
- Description: {DESCRIPTION}

Codebase Context:
- Language/Framework: {TECH_STACK}
- Usage Pattern: {HOW_LIBRARY_IS_USED}
- Exposure: {PUBLIC_FACING_OR_INTERNAL}

Analyze:
1. Is the vulnerable code path exercised in this codebase?
2. What would an attacker need to exploit this?
3. What is the realistic impact if exploited?
4. Are there existing mitigations?
5. What is the contextual risk score (0-10)?

Provide a detailed assessment in JSON format.
```

### 3. Generate Security Report

Create a comprehensive security report following this template:

#### Security Report Template

```markdown
# Security Assessment Report

**Release Version:** v1.0.1
**Assessment Date:** 2026-02-15
**Assessed By:** AI Security Analysis
**Previous Release:** v1.0.0

## Executive Summary

- **Total Dependencies:** 150
- **Vulnerabilities Found:** 5
- **Critical:** 0
- **High:** 1
- **Medium:** 3
- **Low:** 1
- **Risk Status:** ACCEPTABLE / NEEDS ATTENTION / CRITICAL

## CVE Summary

### Fixed CVEs (since v1.0.0)
- CVE-2024-11111: SQL Injection in library-x
- CVE-2024-22222: XSS in component-y

### New CVEs (introduced in v1.0.1)
- CVE-2024-33333: Path Traversal in new-dependency

### Persistent CVEs (carried over from v1.0.0)
- CVE-2024-44444: Information Disclosure (accepted risk)

## Detailed CVE Analysis

### CVE-2024-33333: Path Traversal in new-dependency

**Public Information:**
- **CVSS Score:** 7.5 (High)
- **CWE:** CWE-22 (Path Traversal)
- **Affected:** new-dependency@1.2.3
- **Published:** 2024-12-15
- **References:** 
  - https://nvd.nist.gov/vuln/detail/CVE-2024-33333
  - https://github.com/project/advisory

**AI Contextual Analysis:**
- **Exploitability:** Low
  - Vulnerable function is only used in admin-only endpoints
  - Requires authenticated access with admin privileges
  - Input validation layer exists before reaching vulnerable code
  
- **Impact:** Medium
  - Could potentially read configuration files
  - Cannot write or execute arbitrary code
  - Limited to specific directory structure
  
- **Exposure:** Protected
  - Not accessible to unauthenticated users
  - Admin interface requires 2FA
  - Network access restricted to internal VPN
  
- **Mitigation:** Partial
  - Additional input sanitization in application layer
  - File system permissions limit accessible paths
  - Monitoring alerts on suspicious access patterns

- **Contextual Risk Score:** 2.8 / 10.0
- **Recommendation:** Monitor for patch, acceptable for current release
- **Action Required:** Plan upgrade in next minor version

---

## Comparison with Previous Release

| Metric | v1.0.0 | v1.0.1 | Change |
|--------|---------|---------|--------|
| Total CVEs | 5 | 4 | -1 ↓ |
| Critical | 1 | 0 | -1 ↓ |
| High | 2 | 1 | -1 ↓ |
| Medium | 2 | 3 | +1 ↑ |
| Low | 0 | 0 | 0 |
| Avg CVSS | 6.8 | 5.2 | -1.6 ↓ |
| Avg Contextual Risk | 4.2 | 2.1 | -2.1 ↓ |

**Security Posture:** IMPROVED ✓

## Recommendations

1. **Immediate Actions:**
   - None required

2. **Short-term (Next Minor Release):**
   - Upgrade new-dependency to version 1.2.4 (patches CVE-2024-33333)
   - Review and update input validation for admin endpoints

3. **Long-term:**
   - Establish automated dependency update process
   - Implement security regression testing

## Accepted Risks

### CVE-2024-44444: Information Disclosure
- **Reason:** False positive - vulnerable code path not present in production build
- **Reviewed By:** Security Team
- **Review Date:** 2026-02-10
- **Next Review:** 2026-05-10

## Tools Used

- OWASP Dependency-Check v8.0.0
- Snyk CLI v1.1200.0
- AI Analysis: GPT-4-based contextual assessment

## Sign-off

This security assessment has been completed and reviewed. The release is
approved from a security perspective with the noted acceptable risks.

**Date:** 2026-02-15
**Status:** APPROVED
```

### 4. AI Rules for Security Assessment

When performing a security assessment:

1. **Scan Dependencies**
   ```bash
   # Create version directory
   VERSION="v1.0.1"
   mkdir -p security-reports/$VERSION
   
   # Run dependency scanner
   # (Tool depends on project type)
   ```

2. **Parse Scan Results**
   - Extract all CVEs from scan output
   - Parse JSON reports for structured data
   - Group by severity

3. **For Each CVE:**
   - Fetch public CVE details from NVD or similar
   - Analyze codebase to understand library usage
   - Assess exploitability in context
   - Calculate contextual risk score
   - Document findings in cve-analysis.json

4. **Generate Comparison**
   - Load previous release report
   - Compare CVE lists
   - Identify fixed, new, and persistent CVEs
   - Calculate metrics deltas

5. **Create Report**
   - Generate security-report.md using template
   - Include all CVE analyses
   - Add comparison section
   - Provide actionable recommendations

6. **Update Symlink**
   ```bash
   cd security-reports
   rm -f latest
   ln -s $VERSION latest
   ```

## CVE Analysis JSON Schema

```json
{
  "version": "v1.0.1",
  "assessment_date": "2026-02-15T10:00:00Z",
  "total_dependencies": 150,
  "vulnerabilities": [
    {
      "cve_id": "CVE-2024-33333",
      "cvss_score": 7.5,
      "severity": "HIGH",
      "cwe": "CWE-22",
      "affected_component": "new-dependency@1.2.3",
      "published_date": "2024-12-15",
      "description": "Path traversal vulnerability...",
      "references": [
        "https://nvd.nist.gov/vuln/detail/CVE-2024-33333"
      ],
      "contextual_analysis": {
        "exploitability": "LOW",
        "impact": "MEDIUM",
        "exposure": "PROTECTED",
        "mitigation": "PARTIAL",
        "contextual_risk_score": 2.8,
        "analysis": "Vulnerable function is only used in...",
        "recommendation": "Monitor for patch, acceptable for current release"
      }
    }
  ],
  "comparison": {
    "previous_version": "v1.0.0",
    "fixed_cves": ["CVE-2024-11111", "CVE-2024-22222"],
    "new_cves": ["CVE-2024-33333"],
    "persistent_cves": ["CVE-2024-44444"]
  }
}
```

## Integration with Release Process

1. **Before Tagging Release**
   - Run security scan
   - Generate assessment report
   - Review findings with team
   - Document accepted risks

2. **After Release**
   - Archive security report with version tag
   - Update security documentation
   - Communicate any critical findings

3. **Regular Monitoring**
   - Set up automated scans (weekly/monthly)
   - Monitor for new CVEs in dependencies
   - Track remediation progress

## Tools and Resources

### Recommended Tools

- **OWASP Dependency-Check**: Java, .NET, Ruby, Node.js, Python
- **Snyk**: Multi-language, excellent CI/CD integration
- **npm audit**: Node.js built-in
- **pip-audit**: Python dependencies
- **bundler-audit**: Ruby gems
- **GitHub Dependabot**: Automated alerts and PRs
- **Trivy**: Container and filesystem scanning

### CVE Databases

- National Vulnerability Database (NVD): https://nvd.nist.gov/
- MITRE CVE: https://cve.mitre.org/
- GitHub Advisory Database: https://github.com/advisories
- Snyk Vulnerability DB: https://snyk.io/vuln

### CVSS Calculator

- CVSS v3.1 Calculator: https://www.first.org/cvss/calculator/3.1

## Best Practices

1. **Automate Scanning**: Integrate into CI/CD pipeline
2. **Track Trends**: Monitor security posture over time
3. **Prioritize Contextually**: Not all HIGH CVEs are high risk in your context
4. **Document Decisions**: Record why certain risks are accepted
5. **Regular Reviews**: Re-assess accepted risks periodically
6. **Update Dependencies**: Keep dependencies current
7. **Security Testing**: Complement scanning with penetration testing
8. **Incident Response**: Have a plan for critical vulnerabilities
