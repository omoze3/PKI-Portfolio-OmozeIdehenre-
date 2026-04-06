# Certificate Chain (Lab 03)

This folder contains the TLS certificate chain extracted and organized in trust order:

1. 01-root.pem  
   - Root Certificate Authority (self-signed trust anchor)

2. 02-intermediate.pem  
   - Intermediate CA that signs the server certificate

3. 03-server.pem  
   - Leaf certificate presented by the server

## Notes

- Certificates are ordered to reflect real-world trust validation flow
- Root certificate is typically stored in system trust stores
- This chain was extracted using OpenSSL with SNI enabled

# Week 3 — Certificate Artifacts

This folder contains certificate artifacts generated and analyzed across Week 3 labs.

---

## Certificate Chain Components

### 00-original-leaf.pem
- Extracted leaf certificate from a live endpoint
- Used for inspection of certificate fields and validation properties

### 01-root.pem
- Root Certificate Authority (CA)
- Represents the trust anchor in the certificate chain

### 02-intermediate.pem
- Intermediate CA certificate
- Bridges trust between root and server certificate

### 03-server.pem
- Server certificate signed by intermediate CA
- Used in chain validation and trust verification exercises

---

## Additional Artifacts

### github_cert.pem
- Live certificate retrieved from GitHub
- Used for real-world certificate inspection and validation

---

## Purpose

These artifacts demonstrate:

- Certificate structure and hierarchy
- Trust chain relationships
- Real-world certificate extraction and analysis
- Misconfiguration identification and validation behavior

---

## Key Concept

Identity in PKI is not a single certificate — it is a **chain of trust**.

Each certificate depends on the one above it, ultimately anchored in a trusted root.
