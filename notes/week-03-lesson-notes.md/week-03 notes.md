# Week 03 Lesson Notes — Certificate Chains & Trust in PKI

## 1. Core Concept

Public Key Infrastructure (PKI) uses certificate chains to establish trust between a user and a website. Instead of trusting every website directly, systems rely on a hierarchy of trust that begins with a root Certificate Authority (CA), flows through intermediate CAs, and ends with a leaf (server) certificate.

Each certificate is digitally signed by the one above it, creating a chain that can be verified step-by-step.

---

## 2. Why It Matters

This concept is critical in real enterprise environments because it enables secure communication over HTTPS without users needing to manually verify identities.

- Browsers automatically validate certificate chains when connecting to websites
- Enterprises use internal PKI for secure communication between systems
- Trust chains prevent attackers from impersonating legitimate services

Without certificate chains, secure communication at scale would not be possible.

---

## 3. Technical Breakdown

### Definition
A certificate chain is a sequence of certificates where each certificate is signed by a trusted issuer, ultimately linking back to a root CA.

### Components
- **Root CA**: The top-level trusted authority (self-signed)
- **Intermediate CA**: Issues certificates on behalf of the root
- **Leaf Certificate**: Issued to the actual domain (e.g., github.com)

### Flow
1. Client connects to a server (e.g., github.com)
2. Server presents its certificate (leaf)
3. Server also provides intermediate certificate(s)
4. Client verifies:
   - Leaf is signed by intermediate
   - Intermediate is signed by root
5. Root is already trusted in the system → trust is established

### Trust Implications
- Trust is not direct — it is inherited through the chain
- If any certificate in the chain is invalid, trust fails
- Root CAs must be highly protected because they anchor trust globally

---

## 4. Common Misconceptions

- **“The website itself is trusted directly”**  
  False — trust comes from the certificate chain, not the website alone

- **“The root CA signs every certificate”**  
  False — root CAs typically delegate signing to intermediates for security

---

## 5. Where This Shows Up

- **Web security**  
  HTTPS connections rely on certificate chains for browser trust

- **Internal enterprise systems**  
  Companies use internal CAs to secure APIs, services, and devices

- **Cloud environments**  
  TLS is used for secure communication between microservices

- **DevOps workflows**  
  Certificates are used in CI/CD pipelines, service authentication, and secure deployments

---

## Mental Model

PKI is a system of **Identity + Trust + Verification**:

- Identity = the certificate (who you are)
- Trust = the certificate chain (who vouches for you)
- Verification = cryptographic validation (proof you are legitimate)
