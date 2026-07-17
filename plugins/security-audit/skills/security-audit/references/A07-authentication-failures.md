# A07:2025 — Authentication Failures

Source: https://owasp.org/Top10/2025/A07_2025-Authentication_Failures/

**Rank:** #7 (36 CWEs; 15.80% max incidence; 1,120,673 total occurrences)

## Description

Attackers trick systems into recognizing invalid users as legitimate. Encompasses weak or bypassed authentication mechanisms, poor session management, and hardcoded credentials.

## Audit Checklist — What to Search For

- Hardcoded usernames, passwords, or API keys in source code or config files
- No MFA enforced for admin or high-privilege accounts
- Password policies that don't align with NIST 800-63b (forcing rotation, complexity rules that lead to weak passwords)
- New/changed passwords not checked against breached credential databases (HaveIBeenPwned API)
- No brute-force protection on login endpoints (no lockout, no CAPTCHA, no rate limiting)
- Session IDs that are predictable, low-entropy, or reused across logins
- Session not invalidated on logout (server-side session still valid)
- Session not rotated after privilege escalation / login
- Same error message not used for "user not found" vs "wrong password" (account enumeration)
- SSO sessions with overly long lifetimes and no re-authentication on sensitive actions
- Basic authentication over HTTP (credentials in base64, not encrypted)

## Prevention (for fix suggestions)

- Enforce MFA on all privileged accounts; offer MFA to all users
- Use server-side session managers generating high-entropy (≥128-bit) session IDs
- Rotate session ID immediately after login
- Implement account lockout or exponential backoff after N failed attempts
- Validate new passwords against HaveIBeenPwned or equivalent
- Standardize error messages (do not reveal whether user exists)
- Follow NIST 800-63b: no forced rotation unless breach suspected, no complexity rules
- Use proven auth frameworks (Spring Security, Keycloak) over custom implementations

## Example Attack Patterns

```
# Credential stuffing
for line in leaked_credentials.txt:
    POST /login { username, password }   → find valid accounts

# Session fixation
GET /login?sessionid=attacker-known-id
→ victim logs in, attacker uses the same session ID

# Account enumeration via different error messages
POST /login { user: "admin", pass: "wrong" }   → "Invalid password"
POST /login { user: "nobody", pass: "wrong" }  → "User not found"
→ reveals which users exist
```
