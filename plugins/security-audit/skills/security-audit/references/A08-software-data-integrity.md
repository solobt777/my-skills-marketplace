# A08:2025 — Software or Data Integrity Failures

Source: https://owasp.org/Top10/2025/A08_2025-Software_or_Data_Integrity_Failures/

**Rank:** #8 (14 CWEs; 2.75% average incidence; 501,327 total occurrences)

## Description

Failures to maintain trust boundaries and verify the integrity of software, code, and data artifacts. Covers insecure CI/CD pipelines, untrusted update mechanisms, and insecure deserialization.

## Audit Checklist — What to Search For

**Deserialization:**
- Java `ObjectInputStream.readObject()` receiving data from untrusted sources (HTTP params, cookies, messages)
- `XStream`, `Kryo`, `Jackson` deserialization of untrusted input without type whitelisting
- `.deserialize()` calls on data originating outside the trust boundary

**CI/CD Integrity:**
- No artifact signing or checksum verification in the build pipeline
- Third-party scripts included via CDN without Subresource Integrity (SRI) hashes (`integrity="sha384-..."`)
- Auto-update mechanisms that don't verify signatures before applying updates
- No peer review requirement before merging to main/production branch
- Build pipeline scripts with overly broad permissions or secrets accessible to all jobs

**Data Integrity:**
- Serialized state stored in client-side cookies or URL parameters without HMAC verification
- JWT tokens with `alg: none` accepted
- Unsigned or unencrypted serialized data accepted from untrusted clients

## Prevention (for fix suggestions)

- Use digital signatures to verify software/data is from expected source and unaltered
- Restrict libraries/dependencies to vetted, trusted repositories; consider private artifact mirrors
- Enforce mandatory code review before merging
- Add SRI hashes to all CDN-hosted scripts and stylesheets
- Validate all serialized data — never deserialize from untrusted sources without type checking
- Implement proper CI/CD segregation and access controls (least-privilege pipeline jobs)
- Use HMAC to verify any state stored client-side

## Example Attack Patterns

```java
// VULNERABLE — insecure deserialization
ObjectInputStream ois = new ObjectInputStream(request.getInputStream());
Object obj = ois.readObject();  // attacker controls the byte stream → RCE

// VULNERABLE — CDN script without integrity check
<script src="https://cdn.example.com/lib.js"></script>

// SAFE — with SRI
<script src="https://cdn.example.com/lib.js"
        integrity="sha384-abc123..."
        crossorigin="anonymous"></script>
```
