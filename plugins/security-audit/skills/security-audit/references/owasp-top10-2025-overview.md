# OWASP Top 10:2025 — Overview

Source: https://owasp.org/Top10/2025/

| Code | Category | Key Audit Focus |
|------|----------|-----------------|
| A01 | Broken Access Control | Auth guards, IDOR, CORS, JWT manipulation, forced browsing |
| A02 | Security Misconfiguration | Default creds, debug endpoints, stack traces exposed, directory listing |
| A03 | Software Supply Chain Failures | Outdated deps, no SBOM, unvetted sources, CI/CD integrity |
| A04 | Cryptographic Failures | Weak algorithms, hardcoded keys, no TLS ≥1.2, weak password hashing |
| A05 | Injection | SQL/HQL/OS/LDAP injection via string concatenation |
| A06 | Insecure Design | No rate limiting, unrestricted uploads, security question auth, no threat model |
| A07 | Authentication Failures | No MFA, hardcoded passwords, weak session IDs, no brute-force protection |
| A08 | Software or Data Integrity Failures | No checksum verification, insecure deserialization, unsigned updates |
| A09 | Security Logging and Alerting Failures | Missing auth event logs, sensitive data in logs, no alerting |
| A10 | Mishandling of Exceptional Conditions | Bare catch blocks, fail-open errors, resource leaks on exception |

## Notable Changes vs 2021

- **A03 Software Supply Chain Failures** — new top-level category (was part of A06 2021)
- **A10 Mishandling of Exceptional Conditions** — brand new category in 2025
- **Injection** dropped from #3 to #5
- **Cryptographic Failures** moved from #2 to #4
