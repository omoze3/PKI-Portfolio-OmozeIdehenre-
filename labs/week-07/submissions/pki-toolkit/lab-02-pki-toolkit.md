# My PKI Toolkit
**CVI PKI Career Pathway — Phase 1 Foundations**  
Completed: April 17, 2026

---

## About This Document

This document is my personal PKI reference built from hands-on lab work across 
Weeks 1–7. It captures the tools I actually used, when I used them, and how they helped me diagnose certificate and TLS-related issues.

This toolkit reflects how I approach PKI problems in real scenarios — retrieving certificates, validating trust chains, identifying failures, and understanding how infrastructure impacts security.

---

## Core Command-Line Tools

### openssl x509 — Parse and inspect certificates

**What it does:**  
Reads an X.509 certificate file and displays fields such as subject, issuer, validity dates, SAN entries, and extensions.

**When to use it:**  
I use this after retrieving a certificate to understand whether it is valid, who issued it, and whether it matches the hostname.

**Example command from my labs:**
```bash
openssl x509 -in labs/week-07/submissions/enterprise-analysis/artifacts/enterprise_cert.pem -noout -subject -issuer -dates
What the output tells you:
Shows certificate identity, issuing CA, and whether the certificate is within its validity period.

Phase 1 source: Week 7, Lab 01 — Enterprise Certificate Analysis

openssl s_client — Retrieve live certificates

What it does:
Connects to a TLS endpoint and retrieves the server's certificate chain.

When to use it:
I use this as the first step when diagnosing TLS issues or analyzing a live website’s certificate.

Example command from my labs:
openssl s_client -connect servicenow.com:443 -showcerts 2>/dev/null
What the output tells you:
Shows the full certificate chain being served and whether intermediates are included.


Phase 1 source: Week 7, Lab 01 — Enterprise Certificate Analysis

openssl verify — Validate certificate trust

What it does:
Checks whether a certificate can be validated against a trusted chain.

When to use it:
I use this when diagnosing broken chain issues or verifying if a certificate is trusted.

Example command from my labs:

openssl verify leaf_cert.pem

What the output tells you:
Indicates whether the certificate chain is complete or where validation fails.

Phase 1 source: Week 6, Lab 02 — Broken Chain

openssl req — Generate certificate signing requests

What it does:
Creates a CSR and optionally generates a private key.

When to use it:
I use this when creating a new certificate request to send to a CA.

Example command from my labs:

openssl req -new -key test_key.pem -out test_csr.pem

What the output tells you:
Produces a CSR that includes subject details used by the CA.

Phase 1 source: Week 5, Lab 01 — Generate CSR

openssl ocsp — Check revocation status

What it does:
Queries an OCSP responder to determine whether a certificate has been revoked.

When to use it:
I use this when verifying whether a certificate is still trusted beyond just expiration.

Example command from my labs:

openssl ocsp -issuer issuer_cert.pem -cert leaf_cert.pem -url http://ocsp.example.com -resp_text

What the output tells you:
Returns certificate status such as good, revoked, or unknown.

Phase 1 source: Week 5, Lab 02 — Revocation Status

openssl pkcs12 — Inspect certificate bundles

What it does:
Reads PKCS#12 bundles containing certificates and keys.

When to use it:
I use this when working with bundled certificate formats common in enterprise environments.

Example command from my labs:

openssl pkcs12 -in bundle.p12 -info -noout
What the output tells you:
Displays contents of the bundle, including certificates and keys.

Phase 1 source: Phase 1 certificate handling work

Network and Inspection Tools
curl — Inspect TLS headers and infrastructure

What it does:
Fetches HTTP headers and response information from a server.

When to use it:
I use this to identify infrastructure components like CDNs or proxies involved in TLS.

Example command from my labs:

curl -sI https://www.servicenow.com | grep -i "server\|cf-ray\|x-cache\|via\|x-amz"

What the output tells you:
Reveals whether TLS is terminated by services like Akamai or Cloudflare.

Phase 1 source: Week 7, Lab 01 — Enterprise Certificate Analysis

grep — Filter certificate output

What it does:
Filters command output to extract specific fields.

When to use it:
I use this when focusing on SAN entries or specific certificate extensions.

Example command from my labs:

openssl x509 -in enterprise_cert.pem -noout -text | grep -A10 "Subject Alternative Name"

What the output tells you:
Shows SAN entries to verify hostname coverage.

Phase 1 source: Week 7, Lab 01 — Enterprise Certificate Analysis

Reference and Analysis Services
SSL Labs SSL Server Test — External TLS analysis

What it does:
Performs a full external scan of a server’s TLS configuration.

When to use it:
I use this to validate protocol versions, security posture, and certificate configuration.

Example workflow from my labs:
Entered servicenow.com into SSL Labs and reviewed the results.

What the output tells you:
Shows TLS versions supported, certificate validity, and security rating.

Phase 1 source: Week 7, Lab 01 — Enterprise Certificate Analysis

crt.sh — Certificate Transparency search

What it does:
Searches public CT logs for certificates issued to a domain.

When to use it:
I use this to review certificate history and issuing authorities.

Example workflow from my labs:
Searched servicenow.com on crt.sh.

What the output tells you:
Shows issuance history and certificate lifecycle patterns.

Phase 1 source: Week 7, Lab 01 — Enterprise Certificate Analysis

Phase 1 Skills Summary

Phase 1 taught me how to retrieve, inspect, and validate certificates in real environments. I learned how to diagnose issues such as expired certificates, missing intermediates, and SAN mismatches using a repeatable framework.

These tools helped me understand PKI as a system of trust rather than just files. I now approach certificate issues by analyzing identity, validation paths, and infrastructure behavior. This toolkit reflects how I would troubleshoot real-world TLS and certificate problems in an enterprise environment.

---

DER → PEM conversion (Week 5 + 6 bridge)

### openssl x509 -inform DER — Convert certificate formats

**What it does:**  
Converts certificates from DER format to PEM format.

**When to use it:**  
I use this when a certificate is provided in binary format (.der) and needs to be readable or used in other OpenSSL commands.

**Example command from my labs:**
```bash
openssl x509 -inform DER -in intermediate.der -out issuer_cert.pem

What the output tells you:
Produces a readable PEM certificate that can be used for validation and inspection.

Phase 1 source: Week 6, Lab 02 — Broken Chain


---

##CSR inspection (Week 5)

```md
### openssl req -text -noout — Inspect a CSR

**What it does:**  
Displays the contents of a Certificate Signing Request.

**When to use it:**  
I use this after generating a CSR to verify the subject and requested fields before submitting it to a CA.

**Example command from my labs:**
```bash
openssl req -in test_csr.pem -text -noout

What the output tells you:
Shows subject details and requested extensions that will be included in the certificate.

Phase 1 source: Week 5, Lab 01 — Generate CSR

---

## File inspection (Week 1–2 basics)

```md
### file — Identify certificate file types

**What it does:**  
Determines the format of a file (PEM, DER, text, etc.).

**When to use it:**  
I use this when I’m unsure what format a certificate is in before attempting to parse it.

**Example command from my labs:**
```bash
file intermediate.der

What the output tells you:
Indicates whether the file is binary (DER) or text-based (PEM).

Phase 1 source: Week 5 certificate handling work

---

## Basic inspection command (Week 1–3 habit)

```md
### ls — Inspect directory contents

**What it does:**  
Lists files in a directory.

**When to use it:**  
I used this throughout all labs to verify file locations, artifacts, and outputs.

**Example command from my labs:**
```bash
ls labs/week-06/submissions/02-broken-chain/artifacts

What the output tells you:
Confirms that expected files (certificates, outputs) are present.

Phase 1 source: Weeks 1–7

---

## Trust Chain Concept Tool (Week 2–3 thinking)

```md
### Certificate Chain Analysis — Trust Path Validation

**What it does:**  
Validates the relationship between leaf, intermediate, and root certificates.

**When to use it:**  
I use this when diagnosing trust failures or broken certificate chains.

**Example command from my labs:**
```bash
openssl verify -untrusted issuer_cert.pem leaf_cert.pem

What the output tells you:
Shows whether the certificate chain can be trusted when intermediates are supplied.

Phase 1 source: Week 6, Lab 02 — Broken Chain































