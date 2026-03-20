# Lab 03 — Verify a Certificate Chain

## Overview
This lab explored how certificate chains establish trust in PKI by linking a server certificate to a trusted root authority through an intermediate certificate.

---

## Environment
- OS: macOS
- Terminal: Mac Terminal
- Website used: github.com

---

## Chain Verification Result
server.pem: OK

---
## Observations

1. Which certificate is the root CA?
   The root CA is DigiCert Global Root CA. It is self-signed because its Subject and Issuer are the same.

2. Which is the intermediate CA?
   The intermediate CA is DigiCert TLS RSA SHA256 2020 CA1. It is issued by the root CA and signs the server certificate.

3. Which is the leaf certificate?
   The leaf certificate is issued to github.com and is signed by the intermediate CA.

4. How does the Issuer field connect the chain?
   The Issuer of the server certificate matches the Subject of the intermediate certificate, and the Issuer of the intermediate matches the Subject of the root certificate.

5. Why do intermediate certificates exist?
   They protect the root CA by allowing it to remain secure and offline while still enabling certificate issuance.
## Certificate Roles

| Certificate | Subject | Issuer | CA |
|---|---|---|---|
| server.pem | github.com | Intermediate CA | FALSE |
| intermediate.pem | Intermediate CA | Root CA | TRUE |
| root.pem | Root CA | Root CA | TRUE |

---

## Observations

1. Which certificate is the root CA?
   The root CA is the top-level trusted authority and is self-signed.

2. Which is the intermediate CA?
   The intermediate CA is issued by the root and signs the server certificate.

3. Which is the leaf certificate?
   The leaf certificate is the server certificate issued to github.com.

4. How does the Issuer field connect the chain?
   Each certificate’s Issuer matches the Subject of the certificate above it, forming a chain of trust.

5. Why do intermediate certificates exist?
   They protect the root CA by allowing it to remain secure and offline
while still enabling certificate issuance. 
