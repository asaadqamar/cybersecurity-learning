---
topic: Cryptography Basics — Symmetric, Asymmetric, Hashing
source: TryHackMe — Pre Security + Cyber Security 101
date: 2026-06-10
status: complete
---

# Cryptography Basics

## Why Cryptography Matters in Security

Cryptography is how we protect data in transit and at rest. Every HTTPS connection, every password stored in a database, every JWT token — cryptography is underneath all of it. Understanding it is essential for both attacking and defending systems.

---

## Symmetric Encryption

**One key** — same key encrypts and decrypts.

```
Plaintext → [encrypt with KEY] → Ciphertext → [decrypt with KEY] → Plaintext
```

- Fast and efficient for large amounts of data
- Problem: how do you securely share the key with the other party?
- Examples: AES (Advanced Encryption Standard), DES (old, broken)

**AES** is the current standard. AES-256 means a 256-bit key — essentially unbreakable by brute force with today's hardware.

---

## Asymmetric Encryption

**Two keys** — a public key and a private key. What one encrypts, only the other can decrypt.

```
Sender encrypts with RECIPIENT'S PUBLIC KEY
Recipient decrypts with their PRIVATE KEY
```

- Solves the key-sharing problem — public key can be shared openly
- Slower than symmetric — used to *exchange keys*, not encrypt bulk data
- Examples: RSA, ECC (Elliptic Curve Cryptography)

**How HTTPS uses both:** Asymmetric encryption is used during the TLS handshake to securely exchange a symmetric session key. Then that symmetric key encrypts all the actual traffic. Best of both worlds.

---

## Hashing

Hashing is **one-way**. You cannot reverse a hash back to the original data.

```
"password123" → [SHA-256] → "ef92b778bafe..." (always the same output)
"password124" → [SHA-256] → "completely different hash"
```

- Same input always produces same output (deterministic)
- Different inputs produce different outputs (in theory — collisions are a vulnerability)
- Cannot be reversed
- Used for: storing passwords, verifying file integrity, digital signatures

**Common algorithms:**
| Algorithm | Output Length | Status |
|-----------|--------------|--------|
| MD5 | 128-bit | Broken — don't use for passwords |
| SHA-1 | 160-bit | Weak — deprecated |
| SHA-256 | 256-bit | Secure ✅ |
| SHA-512 | 512-bit | Secure ✅ |
| bcrypt | Variable | Secure ✅ — designed for passwords |

**Salting:** Add a random value to a password before hashing. Prevents two identical passwords from having identical hashes, and defeats pre-computed rainbow table attacks.

---

## How This Connects to My Dev Background

**WordPress stores passwords as bcrypt hashes** — I've manually reset them in the database before without knowing that's what the `$P$` prefix meant. Now I understand why you can't reverse it.

**SSL certificates** — when I install an SSL cert on a WordPress site, the site gets an asymmetric key pair (public + private). The TLS handshake uses RSA (asymmetric) to exchange an AES session key (symmetric), then AES encrypts everything after that. I was doing this blind for years.

**API tokens / JWTs** — JWTs are signed with HMAC (hashing + a secret key) or RSA. The signature proves the token wasn't tampered with. I've built APIs that used JWTs without really understanding why the signature worked.

---

## Questions Still Open

- What exactly is a rainbow table and how does salting defeat it?
- What is ECC and why is it replacing RSA in modern systems?
