# A05:2025 — Injection

Source: https://owasp.org/Top10/2025/A05_2025-Injection/

**Rank:** #5 (37 CWEs; 62,445 CVEs; SQL Injection 14k+ CVEs, XSS 30k+ CVEs; 100% of apps tested)

## Description

Untrusted user input sent to an interpreter (database, OS, browser, LDAP) causes the interpreter to execute portions of the input as commands. Occurs when applications fail to validate, filter, or sanitize user-supplied data before using it in dynamic queries or commands.

## Audit Checklist — What to Search For

**SQL / HQL Injection:**
- String concatenation in queries: `"SELECT ... WHERE id='" + param + "'"`
- `createQuery()` / `createNativeQuery()` with concatenated user input
- Hibernate HQL built dynamically without parameterization

**OS Command Injection:**
- `Runtime.getRuntime().exec()` with user-supplied data
- `ProcessBuilder` with unsanitized arguments

**LDAP Injection:**
- LDAP search filters built with string concatenation from user input

**XSS (Cross-Site Scripting):**
- User input rendered in HTML/JS without escaping
- `innerHTML`, `document.write`, or `eval()` with user data
- Thymeleaf `th:utext` (unescaped) with user-controlled data

**NoSQL Injection:**
- MongoDB queries built with user-controlled keys or operators

**Expression Language Injection:**
- Spring EL or OGNL expressions evaluated from user input

## Prevention (for fix suggestions)

- Use parameterized queries / prepared statements for all DB access
- Use ORM safely — never concatenate into HQL/JPQL
- Use positive server-side input validation (allowlist, not denylist)
- Escape special characters using interpreter-specific escaping
- Use SAST/DAST tools in CI/CD (SonarQube, Checkmarx, OWASP ZAP)
- Structural elements (table/column names) must never accept user input

## Example Attack Patterns

```java
// VULNERABLE — SQL injection
String query = "SELECT * FROM accounts WHERE custID='" + request.getParameter("id") + "'";
// Attack: id = ' OR '1'='1   → returns all accounts

// VULNERABLE — HQL injection
Query q = session.createQuery("FROM accounts WHERE custID='" + request.getParameter("id") + "'");

// VULNERABLE — OS command injection
String cmd = "nslookup " + request.getParameter("domain");
Runtime.getRuntime().exec(cmd);
// Attack: domain = example.com; cat /etc/passwd

// SAFE — parameterized
PreparedStatement ps = conn.prepareStatement("SELECT * FROM accounts WHERE custID = ?");
ps.setString(1, request.getParameter("id"));
```
