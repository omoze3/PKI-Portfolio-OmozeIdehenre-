# Lab 02 — Investigate Certificate Extensions

## Overview
This lab focused on analyzing X.509 certificate extensions to understand how certificates define their usage, validation rules, and trust behavior in a PKI system.

---

## Environment
- OS: macOS
- Terminal used: Mac Terminal
- OpenSSL version: 3.3.6

---

## Extensions Found

### Subject Alternative Name (SAN)
DNS:*.google.com, DNS:google.com, DNS:youtube.com, DNS:*.youtube.com, DNS:g.co, DNS:*.g.co, DNS:*.gstatic.com and many other Google-related domains

### Key Usage
Digital Signature

### Extended Key Usage (EKU)
TLS Web Server Authentication

### Basic Constraints
CA: FALSE

---

## Observations

1. What domains appear in the SAN field?
   The SAN field includes multiple Google-related domains such as *.google.com, google.com, youtube.com, and other Google service domains.

2. What operations are permitted by Key Usage?
   The certificate is allowed to perform digital signatures, which are used in TLS handshakes to verify identity and integrity.

3. What applications are authorized by EKU?
   The certificate is authorized for TLS Web Server Authentication, meaning it can be used to secure HTTPS connections.

4. Can this certificate issue other certificates? How do you know?
   No. The Basic Constraints field shows CA:FALSE, which means it is not a Certificate Authority.

5. Why are these extensions important for TLS validation?
   These extensions tell systems what the certificate is valid for, what domains it covers, and whether it can act as a CA. They help browsers and clients reject certificates that are being misused.
