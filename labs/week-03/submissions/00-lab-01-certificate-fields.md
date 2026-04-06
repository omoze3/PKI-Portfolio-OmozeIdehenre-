# Lab 01 — Inspect X.509 Certificate Fields

## Overview
This lab focused on retrieving and analyzing an X.509 certificate using OpenSSL to understand how PKI establishes trust.

---

## Environment
- OS: macOS
- Terminal used: Mac Terminal
- OpenSSL version:(run `openssl version`)3.3.6

---

## Certificate Fields

| Field                | Value from your output |
|----------------------|------------------------|
| Version              |  Version 3                      |
| Serial Number        |  aa:23:02:42:8e:f4:39:7e:10:bb:2c:32:93:1c:fc:2e                      |
| Signature Algorithm  |  ecdsa-with-SHA256                      |
| Issuer               |  C=US, O=Google Trust Services, CN=WE2                     |
| Subject              |  CN=*.google.com                      |
| Not Before           |  Feb 23 18:19:56 2026 GMT                      |
| Not After            |  May 18 18:19:55 2026 GMT                      |
| Public Key Algorithm |  id-ecPublicKey                      |

<img width="553" height="467" alt="Google Certificate" src="https://github.com/user-attachments/assets/b49c129c-a810-4f4f-bd9d-28a0e4c54703" />

---

## Observations

1. The certificate was issued by:
   Google Trust Services (Certificate Authority WE2).

2. It represents the domain:
   *.google.com, meaning it secures Google and its subdomains.

   
3. The certificate expires on:
   May 18, 2026.

4. The public key algorithm used is:
   Elliptic Curve Cryptography (ECC), specifically id-ecPublicKey using the P-256 curve.

5. The Issuer field matters because it identifies the trusted Certificate Authority that verifies the website’s identity.
In a PKI system, trust is established because systems trust recognized CAs to validate and sign certificates, ensuring secure and authentic communication.
