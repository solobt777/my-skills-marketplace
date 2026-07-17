# Security Audit Report Template

Use this template when writing the report to `audit/<YYYY-MM-DD>/security-report.md`.

---

```markdown
# Security Audit Report

**Date:** YYYY-MM-DD
**Project:** <project name>
**Auditor:** Claude Code — OWASP Top 10:2025
**Scope:** All modules / subprojects in the repository

---

## Executive Summary

| Severity | Count |
|----------|-------|
| Critical | X |
| High | X |
| Medium | X |
| Low | X |
| Informational | X |
| **Total** | **X** |

<2-3 sentence summary of the most critical issues and overall security posture.>

---

## Findings

### [CRITICAL/HIGH/MEDIUM/LOW/INFO] A0X — <Category Name>

**File:** `path/to/file.java:42`
**Description:** What was found and why it is a vulnerability.
**Risk:** What an attacker could do by exploiting this.
**Suggested Fix:**

```java
// Example of the fix in context
```

---

### [SEVERITY] A0X — <Category Name>

*(repeat for each finding)*

---

## Categories with No Findings

- A0X — <Category Name>: No issues detected
- A0X — <Category Name>: No issues detected

---

## Recommendations (Priority Order)

1. **Critical items first** — address before next release
2. **High items** — address within current sprint
3. **Medium items** — address within next 2 sprints
4. **Low/Informational** — track in backlog

---

## References

- OWASP Top 10:2025 — https://owasp.org/Top10/2025/
- A01 Broken Access Control — https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/
- A02 Security Misconfiguration — https://owasp.org/Top10/2025/A02_2025-Security_Misconfiguration/
- A03 Software Supply Chain — https://owasp.org/Top10/2025/A03_2025-Software_Supply_Chain_Failures/
- A04 Cryptographic Failures — https://owasp.org/Top10/2025/A04_2025-Cryptographic_Failures/
- A05 Injection — https://owasp.org/Top10/2025/A05_2025-Injection/
- A06 Insecure Design — https://owasp.org/Top10/2025/A06_2025-Insecure_Design/
- A07 Authentication Failures — https://owasp.org/Top10/2025/A07_2025-Authentication_Failures/
- A08 Software or Data Integrity — https://owasp.org/Top10/2025/A08_2025-Software_or_Data_Integrity_Failures/
- A09 Security Logging — https://owasp.org/Top10/2025/A09_2025-Security_Logging_and_Alerting_Failures/
- A10 Exceptional Conditions — https://owasp.org/Top10/2025/A10_2025-Mishandling_of_Exceptional_Conditions/
```
