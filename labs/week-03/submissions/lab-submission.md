# Lab — Certificate Chain & Live Certificate Analysis

## Overview
This lab focused on retrieving and analyzing TLS certificates from live websites using OpenSSL. The goal was to understand how certificate chains are structured, how trust is established, and how certificate fields and extensions support secure HTTPS communication.

---

## Environment

- Operating System: macOS
- Terminal Used: Mac Terminal
- OpenSSL Version: OpenSSL 3.x
- Websites Used: github.com, google.com

---

## Steps Performed

1. Used OpenSSL to connect to a live website and retrieve its certificate chain:
   openssl s_client -connect github.com:443 -showcerts

2. Copied and separated the certificate chain into individual files:
   - Leaf (server certificate)
   - Intermediate certificate

3. Saved certificates locally as `.pem` files and inspected them using:
   openssl x509 -in <file>.pem -text -noout

4. Reviewed key certificate fields including:
   - Subject
   - Issuer
   - Validity dates
   - Subject Alternative Names (SAN)
   - Key Usage and Extended Key Usage

5. Compared certificate extensions between different websites (GitHub vs Google).

---

## Results

- Successfully retrieved a full certificate chain from a live HTTPS connection
- Identified:
  - Leaf certificate (issued to github.com)
  - Intermediate certificate (issued by a trusted CA)
- Verified TLS connection details:
  - Protocol: TLSv1.3
  - Cipher: AES-GCM

- Observed key certificate fields:
  - Subject: CN=github.com
  - Issuer: Sectigo ECC Domain Validation Secure Server CA
  - SAN entries: github.com, www.github.com
  - Extended Key Usage: TLS Web Server Authentication

---

## Key Findings

- Certificates are structured in a chain: leaf → intermediate → root
- The leaf certificate identifies the website being accessed
- The intermediate CA signs the leaf certificate and links it to a trusted root
- SAN (Subject Alternative Name) allows a single certificate to secure multiple domains
- Extended Key Usage defines how the certificate is allowed to be used (e.g., HTTPS)

---

## Explanation

These results demonstrate how PKI enables secure communication over HTTPS.

- The certificate chain allows browsers to verify trust without directly exposing the root CA
- The Issuer and Subject fields connect certificates in the chain
- SAN entries ensure the certificate matches the domain being accessed
- Extended Key Usage ensures the certificate is valid specifically for TLS server authentication

In real-world systems, this process is what allows users to securely access websites without manually verifying certificates.

---

## Challenges / Troubleshooting

- Initially had issues saving and organizing certificate files due to incorrect directory paths
- Encountered confusion separating certificate chain blocks into individual `.pem` files
- Resolved by carefully copying each certificate block and saving them individually
- Also fixed Git issues related to file structure and pushing changes to the repository

---

## Artifacts

- github_cert.pem (leaf certificate)
- intermediate.pem (intermediate certificate)
- lab-03-certificate-chain.md (lab write-up)
- lab-05-extract-live-certificate.md (live certificate analysis)
- Terminal outputs from OpenSSL commands

---

*CVI PKI Career Pathway — Foundations Phase*
