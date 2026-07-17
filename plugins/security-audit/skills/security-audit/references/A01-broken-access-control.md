# A01:2025 — Broken Access Control

Source: https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/

**Rank:** #1 (highest occurrence — 1,839,701 instances documented, 40 CWEs mapped)

## Description

Access control mechanisms enforce policy boundaries preventing users from operating beyond their authorized scope. Failures enable unauthorized data disclosure, modification, destruction, or execution of restricted business functions.

## Audit Checklist — What to Search For

- Endpoints that skip authentication checks (look for routes with no `@PreAuthorize`, `@Secured`, or filter chain coverage)
- URL parameter manipulation enabling access to other users' data (e.g., `?userId=123` with no ownership check)
- Insecure direct object references — predictable IDs used in queries without authorization check
- Missing protection on mutating API methods (POST, PUT, DELETE) that only restrict GET
- CORS configured with wildcard `*` origins on sensitive endpoints
- JWT/session tokens not validated server-side (client-side-only checks)
- Admin or privileged functionality accessible without role check
- Directory listing enabled on web server
- `.git/`, backup files, or metadata accessible from web root

## Prevention (for fix suggestions)

- Default-deny: all non-public resources require explicit permission grant
- Implement access control server-side; never trust client-side enforcement
- Use centralized, reusable access control mechanisms (Spring Security `@PreAuthorize`, filter chains)
- Enforce record ownership through domain model, not just role
- Log access control failures with alerting on repeated failures
- Apply rate limiting on APIs

## Example Attack Patterns

```
# IDOR — parameter tampering
GET /app/accountInfo?acct=notmyacct

# Forced browsing — unauthenticated admin access
GET /app/admin_getappInfo

# Direct API call bypassing browser-based controls
curl https://example.com/app/admin_getappInfo
```
