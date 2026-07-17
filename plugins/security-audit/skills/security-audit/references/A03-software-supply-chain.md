# A03:2025 — Software Supply Chain Failures

Source: https://owasp.org/Top10/2025/A03_2025-Software_Supply_Chain_Failures/

**Rank:** #3 (new top-level category; highest average incidence 5.72%; #1 in community survey)

## Description

Breakdowns or compromises in the process of building, distributing, or updating software — through vulnerabilities or malicious alterations in third-party dependencies, tools, and components.

## Audit Checklist — What to Search For

- `pom.xml`, `package.json`, `build.gradle` — dependencies with known CVEs (use `mvn dependency:check`, `npm audit`, OWASP Dependency-Check)
- Dependencies that are outdated, end-of-life, or unsupported
- No version pinning — using floating ranges (`^`, `~`, `latest`, `RELEASE`)
- No SBOM (Software Bill of Materials) generation in the build pipeline
- Components sourced from unofficial registries or mirrors
- CI/CD pipeline lacks integrity checks (no artifact signing, no checksum verification)
- Weak separation of duties — single person can push directly to production
- No MFA on developer workstations, build systems, or artifact registries
- No monitoring for newly discovered CVEs in existing dependencies

## Prevention (for fix suggestions)

- Add OWASP Dependency-Check or Dependency-Track to CI pipeline
- Pin exact versions; use lock files (package-lock.json, poetry.lock)
- Generate and maintain SBOM (CycloneDX or SPDX format)
- Obtain components exclusively from official trusted sources
- Require signed artifacts; verify checksums before use
- Enforce code review + segregation of duties in CI/CD
- Enable MFA on all developer and build infrastructure accounts
- Subscribe to CVE/NVD feeds and vendor security bulletins

## Notable Real-World Examples

- **SolarWinds 2019** — compromised updates distributed to ~18,000 organizations
- **Log4Shell (CVE-2021-44228)** — RCE via Log4j component used across thousands of apps
- **Struts 2 (CVE-2017-5638)** — RCE via Apache Struts component
