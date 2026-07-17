# A09:2025 — Security Logging and Alerting Failures

Source: https://owasp.org/Top10/2025/A09_2025-Security_Logging_and_Alerting_Failures/

**Rank:** #9 (CWEs: CWE-117, CWE-532, CWE-778)

## Description

Without logging and monitoring, attacks and breaches cannot be detected, and without alerting it is very difficult to respond quickly. Encompasses inadequate event logging, insufficient monitoring, missing detection capabilities, and ineffective alerting.

## Audit Checklist — What to Search For

- Authentication events not logged (successful logins, failed logins, logouts)
- Authorization failures not logged (access denied events)
- No logging of admin/privileged operations (user creation, role changes, config changes)
- Sensitive data written to logs (passwords, credit card numbers, PII, session tokens, API keys)
- Log format incompatible with log management tools (SIEM/ELK/Splunk) — e.g., unstructured plain text
- Log data not properly encoded — potential log injection (CWE-117): newlines, ANSI codes in log messages from user input
- Logs stored locally only with no shipping to centralized, append-only storage
- No alerting configured for repeated authentication failures or suspicious patterns
- No monitoring use cases or runbooks defined for security events
- Application errors that silently swallow exceptions without logging

## Prevention (for fix suggestions)

- Log all authentication events (success + failure) with user context, timestamp, IP
- Log all security control activities — both successes and failures
- Produce logs in structured format (JSON) compatible with SIEM tools
- Encode all user-derived data before writing to logs (prevent log injection)
- Implement append-only audit trails with tamper protection
- Define monitoring and alerting use cases with documented response playbooks
- Deploy honeytokens to detect unauthorized access with low false-positive rate
- Ensure error conditions trigger appropriate alerts and transaction rollbacks

## Example Real-World Incidents

- **7-year breach undetected:** Health plan provider — 3.5 million children's records compromised; no monitoring in place
- **10+ year breach:** Major airline discovered breach only after external notification; data at cloud provider
- **GDPR fine £20M:** European airline — 400,000 customer payment records breached

## Log Injection Example

```java
// VULNERABLE — attacker can inject fake log entries
String username = request.getParameter("username");
logger.warn("Login failed for user: " + username);
// Attack: username = "admin\nINFO: Login succeeded for user: admin"

// SAFE — encode before logging
logger.warn("Login failed for user: {}", username.replaceAll("[\r\n]", "_"));
```
