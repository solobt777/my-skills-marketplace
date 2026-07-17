# A06:2025 — Insecure Design

Source: https://owasp.org/Top10/2025/A06_2025-Insecure_Design/

**Rank:** #6 (39 CWEs; 22.18% max incidence rate)

## Description

Missing or ineffective security control design — architectural flaws and business logic vulnerabilities that emerge from inadequate threat assessment and insufficient security planning during the design phase. A secure implementation cannot fix an insecure design.

## Audit Checklist — What to Search For

- Authentication via security questions (NIST 800-63b non-compliant)
- No rate limiting on sensitive endpoints (login, registration, password reset, OTP)
- No anti-automation / CAPTCHA on high-value flows
- Unrestricted file uploads (no type validation, no size limits, files served from same origin)
- Business logic that can be abused: bulk operations, quantity overrides, price manipulation
- Missing tenant/user data segregation in multi-tenant systems
- Trust boundary violations — user input trusted at internal service boundaries
- Sensitive operations with no secondary confirmation (e.g., fund transfer without re-auth)
- No fraud detection or anomaly detection for critical business flows

## Prevention (for fix suggestions)

- Conduct threat modeling for critical features (authentication, access control, business logic)
- Embed security requirements in user stories and acceptance criteria
- Implement rate limiting on all authentication, registration, and OTP endpoints
- Validate file uploads: check magic bytes (not just extension), restrict MIME types, scan content, store outside web root
- Design robust tenant segregation at data and API layers
- Require secondary confirmation (re-authentication, step-up auth) for high-risk operations
- Build and use libraries of pre-approved secure design patterns

## Example Attack Patterns

```
# No rate limiting on OTP endpoint
POST /api/verify-otp  { "otp": "000000" }
POST /api/verify-otp  { "otp": "000001" }
... (10,000 requests) → brute force OTP

# Cinema booking — no business logic protection
POST /api/book  { "seats": 600, "locations": ["NYC", "LA", "CHI"] }
→ No deposit charged, entire inventory locked

# File upload bypass — polyglot file
Upload image.jpg that is also valid JavaScript → executed if served from same origin
```
