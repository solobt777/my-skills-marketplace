# A02:2025 — Security Misconfiguration

Source: https://owasp.org/Top10/2025/A02_2025-Security_Misconfiguration/

**Rank:** #2 (moved up from #5; 100% of applications contained some form; 719,000+ CWE occurrences)

## Description

Security misconfiguration occurs when a system, application, or cloud service is set up incorrectly from a security perspective, creating vulnerabilities. Common in highly configurable environments deployed without hardening.

## Audit Checklist — What to Search For

- Default credentials left unchanged (admin/admin, default DB passwords)
- Sample or demo applications deployed to production
- Debug endpoints enabled in production (`/actuator`, `/h2-console`, Swagger UI without auth)
- Detailed error messages exposing stack traces, framework versions, or DB structure
- Directory listing enabled on the web server
- Unnecessary features, ports, services, or accounts enabled
- Security headers missing (CSP, X-Frame-Options, HSTS, X-Content-Type-Options)
- Cloud storage buckets/blobs set to public by default
- Hardcoded credentials or secrets in configuration files committed to source control
- Environment-specific configs leaking into non-matching environments (dev config in prod)

## Prevention (for fix suggestions)

- Establish repeatable hardening processes for all environments (dev, QA, prod)
- Deploy minimal platforms — remove unused features, components, sample apps
- Integrate configuration reviews into patch management cycles
- Transmit security directives via security headers
- Use platform-provided identity federation and short-lived credentials over hardcoded secrets
- Automate configuration verification across environments

## Example Attack Patterns

```
# Default credentials
POST /admin/login  body: username=admin&password=admin

# Debug endpoint exposed
GET /actuator/env   → exposes all env vars including secrets

# Stack trace reveals internals
GET /app?id=x'      → returns SQL error with table structure
```
