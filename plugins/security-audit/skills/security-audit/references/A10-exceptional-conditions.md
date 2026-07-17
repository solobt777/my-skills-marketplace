# A10:2025 — Mishandling of Exceptional Conditions

Source: https://owasp.org/Top10/2025/A10_2025-Mishandling_of_Exceptional_Conditions/

**Rank:** #10 (new category in 2025; 24 CWEs)

## Description

Applications fail to prevent, detect, or properly respond to abnormal situations — crashes, unexpected behavior, and security vulnerabilities caused by improper error handling, logical errors, and insecure failure modes.

## Root Causes

- Insufficient input validation
- Late-stage or missing error handling
- Unexpected environmental states (memory exhaustion, privilege issues, network failures)
- Inconsistent exception handling across the codebase

## Audit Checklist — What to Search For

- Bare `catch` blocks that swallow exceptions silently: `catch (Exception e) {}`
- `catch` blocks that log but don't handle or re-throw appropriately
- Fail-open error handling: when an error occurs, the system grants access or continues processing instead of failing safely
- Resource leaks on exception paths — streams, connections, file handles not closed in `finally`/try-with-resources
- Database error messages or stack traces exposed to the user/client
- No global/top-level exception handler for unexpected conditions
- Partial transaction commits on error — database left in inconsistent state
- No rollback on multi-step operations that fail midway
- No resource quotas, rate limiting, or throttling — resource exhaustion via repeated failed operations
- Exception paths that bypass security checks (auth, validation) due to control flow

## Prevention (for fix suggestions)

- Catch every system error at the point it occurs and handle meaningfully (log, alert, throw)
- Implement global exception handlers for unforeseen issues
- Always roll back entire transactions on error — never attempt mid-transaction recovery
- Use fail-closed patterns: on error, deny access / stop processing
- Use try-with-resources to prevent resource leaks
- Apply rate limiting, resource quotas, and throttling
- Centralize error handling, logging, and monitoring
- Perform strict input validation with sanitization for hazardous characters
- Never expose internal error details (stack traces, SQL errors) to end users

## Example Attack Patterns

```java
// VULNERABLE — resource leak on exception
InputStream is = new FileInputStream(path);
process(is);  // exception here → stream never closed → resource exhaustion

// SAFE — try-with-resources
try (InputStream is = new FileInputStream(path)) {
    process(is);
}

// VULNERABLE — fail-open
try {
    checkAuthorization(user);
} catch (Exception e) {
    // swallowed → user proceeds without authorization check
}

// SAFE — fail-closed
try {
    checkAuthorization(user);
} catch (Exception e) {
    logger.error("Auth check failed", e);
    throw new SecurityException("Authorization check failed");
}

// VULNERABLE — transaction not rolled back
db.execute("UPDATE account SET balance = balance - 100 WHERE id = ?", fromId);
// network failure here → money deducted but never credited
db.execute("UPDATE account SET balance = balance + 100 WHERE id = ?", toId);
```
