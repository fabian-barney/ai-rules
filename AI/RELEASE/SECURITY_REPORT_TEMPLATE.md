# Security Assessment Report Template

<!-- 
This is a template for security assessment reports. Copy this file to 
security-reports/vX.Y.Z/security-report.md and fill in the details.
-->

**Release Version:** vX.Y.Z
**Assessment Date:** YYYY-MM-DD
**Assessed By:** [Name/Team]
**Previous Release:** vX.Y.Z-1

## Executive Summary

- **Total Dependencies:** [NUMBER]
- **Vulnerabilities Found:** [NUMBER]
  - **Critical:** [NUMBER]
  - **High:** [NUMBER]
  - **Medium:** [NUMBER]
  - **Low:** [NUMBER]
- **Risk Status:** [ACCEPTABLE / NEEDS ATTENTION / CRITICAL]

**Overall Assessment:** [Brief 2-3 sentence summary of security posture]

## CVE Summary

### Fixed CVEs (since vX.Y.Z-1)

- CVE-YYYY-NNNNN: [Brief description]
- CVE-YYYY-NNNNN: [Brief description]

### New CVEs (introduced in vX.Y.Z)

- CVE-YYYY-NNNNN: [Brief description]

### Persistent CVEs (carried over from vX.Y.Z-1)

- CVE-YYYY-NNNNN: [Brief description] - [Reason for acceptance]

## Detailed CVE Analysis

### CVE-YYYY-NNNNN: [Vulnerability Title]

**Public Information:**

- **CVSS Score:** [0.0-10.0] ([Severity])
- **CVSS Vector:** CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H
- **CWE:** CWE-XXX ([Weakness Type])
- **Affected Component:** [library-name@version]
- **Published Date:** YYYY-MM-DD
- **Description:** [Official CVE description]
- **References:**
  - [NVD Link]
  - [Advisory Link]
  - [Patch Link]

**AI Contextual Analysis:**

- **Exploitability in This Codebase:** [Not Exploitable / Low / Medium / High / Critical]
  - [Detailed analysis of whether and how this vulnerability can be exploited
    in the specific context of this codebase]
  - [Describe attack vectors, prerequisites, and barriers]

- **Impact Assessment:** [Negligible / Low / Medium / High / Critical]
  - [Analysis of what would happen if exploited]
  - [Affected systems, data, or functionality]
  - [Potential blast radius]

- **Exposure Level:** [Internal / Protected / Exposed / Public]
  - [How accessible is the vulnerable component?]
  - [What authentication/authorization protects it?]
  - [Network topology considerations]

- **Mitigation Status:** [None / Partial / Complete]
  - [Existing controls that reduce risk]
  - [Workarounds in place]
  - [Compensating security measures]

- **Contextual Risk Score:** [0.0-10.0]
  - **Calculation Rationale:** [Explain how public CVSS was adjusted based on
    above factors]
  - **Adjustment Factors:**
    - Exploitability: [Impact on score]
    - Impact: [Impact on score]
    - Exposure: [Impact on score]
    - Mitigation: [Impact on score]

**Recommendation:** [IMMEDIATE ACTION / PLAN UPGRADE / MONITOR / ACCEPT RISK]

**Action Required:**

- [ ] [Specific action item 1]
- [ ] [Specific action item 2]
- [ ] [Review date for accepted risks: YYYY-MM-DD]

---

> **Note:** Repeat the above section for each CVE

---

## Comparison with Previous Release

| Metric                | vX.Y.Z-1 | vX.Y.Z | Change       |
|-----------------------|----------|--------|--------------|
| Total Dependencies    | [N]      | [N]    | [±N]         |
| Total CVEs            | [N]      | [N]    | [±N] ↑/↓     |
| Critical              | [N]      | [N]    | [±N] ↑/↓     |
| High                  | [N]      | [N]    | [±N] ↑/↓     |
| Medium                | [N]      | [N]    | [±N] ↑/↓     |
| Low                   | [N]      | [N]    | [±N] ↑/↓     |
| Avg Public CVSS       | [X.X]    | [X.X]  | [±X.X] ↑/↓   |
| Avg Contextual Risk   | [X.X]    | [X.X]  | [±X.X] ↑/↓   |

**Security Posture:** [IMPROVED ✓ / DEGRADED ✗ / UNCHANGED →]

**Analysis:** [Brief explanation of trends and significant changes]

## Recommendations

### Immediate Actions (Critical/High Risk)

1. [Action item with clear owner and deadline]
2. [Action item with clear owner and deadline]

### Short-term (Next Minor/Patch Release)

1. [Action item for near-term addressing]
2. [Action item for near-term addressing]

### Long-term (Strategic Improvements)

1. [Action item for process/architecture improvement]
2. [Action item for process/architecture improvement]

## Accepted Risks

### CVE-YYYY-NNNNN-2: [Vulnerability Title 2]

- **Risk Level:** [High/Medium/Low]
- **Reason for Acceptance:** [Detailed justification]
- **Compensating Controls:** [What mitigates this risk]
- **Reviewed By:** [Name/Role]
- **Review Date:** YYYY-MM-DD
- **Next Review Date:** YYYY-MM-DD
- **Acceptance Criteria:** [Conditions under which this decision should be revisited]

## Dependency Analysis

### New Dependencies Added

| Dependency   | Version    | Purpose    | Security Notes |
|--------------|------------|------------|----------------|
| [name]       | [version]  | [purpose]  | [any concerns] |

### Dependencies Updated

| Dependency   | Old Version | New Version | Reason   | Security Impact |
|--------------|-------------|-------------|----------|-----------------|
| [name]       | [old]       | [new]       | [reason] | [impact]        |

### Dependencies Removed

| Dependency   | Version    | Reason   | Security Impact |
|--------------|------------|----------|-----------------|
| [name]       | [version]  | [reason] | [impact]        |

## Security Testing Performed

- [ ] OWASP Dependency-Check scan
- [ ] npm/pip/etc audit
- [ ] Snyk vulnerability scan
- [ ] Static Application Security Testing (SAST)
- [ ] Dynamic Application Security Testing (DAST)
- [ ] Container/image scanning
- [ ] Infrastructure as Code (IaC) scanning
- [ ] Secret scanning
- [ ] License compliance check

**Tool Versions:**

- OWASP Dependency-Check: [version]
- Snyk: [version]
- Other security tools: [versions]

## Additional Security Notes

[Any additional security-related observations, warnings, or context that doesn't
fit in the above categories]

## Compliance and Regulatory Considerations

[Note any compliance requirements (GDPR, HIPAA, SOC2, etc.) and how this release
affects compliance posture]

## Security Contacts

- **Security Team:** [email]
- **On-Call:** [contact method]
- **Vulnerability Reports:** [security@example.com]

## Approval and Sign-off

This security assessment has been completed and reviewed. The release is:

- [ ] **APPROVED** - Security posture is acceptable
- [ ] **APPROVED WITH CONDITIONS** - Acceptable with noted risks
- [ ] **REJECTED** - Security concerns must be addressed before release

**Security Reviewer:** ___________________
**Date:** ___________________
**Engineering Manager:** ___________________
**Date:** ___________________

---

## Appendix: Vulnerability Details

### Scan Results

Attach or link to:

- [ ] dependency-check-report.html
- [ ] dependency-check-report.json
- [ ] npm-audit.json / pip-audit.json / etc.
- [ ] snyk-report.json
- [ ] cve-analysis.json

### Change Log

| Date       | Version | Reviewer | Changes                  |
|------------|---------|----------|--------------------------|
| YYYY-MM-DD | [ver]   | [name]   | Initial assessment       |
| YYYY-MM-DD | [ver]   | [name]   | Updated after review     |
