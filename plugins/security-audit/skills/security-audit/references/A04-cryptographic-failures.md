# A04:2025 — Cryptographic Failures

Source: https://owasp.org/Top10/2025/A04_2025-Cryptographic_Failures/

**Rank:** #4 (32 CWEs mapped; 13.77% max incidence rate)

## Description

Failures stemming from absent or inadequate cryptography, weak algorithms, compromised keys, or insufficient entropy. Covers data in transit and at rest.

## Audit Checklist — What to Search For

- HTTP (not HTTPS) used for any data transmission — search for `http://` in config files
- TLS version below 1.2 configured (`TLSv1`, `TLSv1.1`, `SSLv3`)
- Weak or deprecated algorithms: MD5, SHA-1, DES, 3DES, RC4, CBC mode without MAC
- Hardcoded cryptographic keys, passwords, or secrets in source code
- Passwords stored as plain text or with unsalted/weak hashes (MD5, SHA1, SHA256 without salt)
- Missing PBKDF2, bcrypt, scrypt, Argon2, or yescrypt for password hashing
- Reused initialization vectors (IVs) for encryption
- Encryption without authentication (unauthenticated modes like ECB, CBC without HMAC)
- Random number generation using `java.util.Random` instead of `SecureRandom`
- Sensitive data (PII, financial, health) stored without encryption at rest
- Keys stored in code repositories, application properties files, or unprotected config

## Prevention (for fix suggestions)

- Use TLS ≥ 1.2 with forward secrecy ciphers only
- Store keys in hardware HSM or cloud key management (AWS KMS, Azure Key Vault)
- Use Argon2, yescrypt, scrypt, or PBKDF2-HMAC-SHA-512 for password hashing
- Use cryptographically secure random IVs, never reuse for a fixed key
- Implement authenticated encryption (AES-GCM, ChaCha20-Poly1305)
- Classify sensitive data and avoid storing what isn't needed
- Begin post-quantum cryptography readiness planning (target: 2030)

## Example Attack Patterns

```
# Downgrade attack on weak TLS — intercept and strip to HTTP
# Attacker on public WiFi captures unencrypted session cookie

# Rainbow table attack on unsalted MD5 password hash
SELECT password FROM users WHERE username='admin'
→ returns: 5f4dcc3b5aa765d61d8327deb882cf99  (MD5 of "password")
```
