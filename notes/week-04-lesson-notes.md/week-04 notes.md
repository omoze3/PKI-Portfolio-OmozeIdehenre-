# Week 04 Lesson Notes — Public Key Infrastructure (PKI) & Certificate Trust

## 1. Core Concept

Public Key Infrastructure (PKI) is the system used to establish trust between parties in a digital environment using certificates and cryptography.

At its core, PKI answers one question:
**“Can I trust who I’m communicating with?”**

This week focused on how certificates are created, how trust is established through root Certificate Authorities (CAs), and how systems validate or reject that trust.

---

## 2. Why It Matters

PKI is foundational to modern security and appears in nearly every enterprise system:

- Every HTTPS website relies on PKI to establish secure connections
- Enterprises use internal root CAs to secure and monitor internal traffic
- Applications and software updates are signed using certificates
- Cloud platforms use certificates for identity, encryption, and authentication

Without PKI, secure communication on the internet and within organizations would not be possible.

---

## 3. Technical Breakdown

### Definition
PKI is a framework that uses asymmetric cryptography and digital certificates to verify identity and establish trust.

---

### Components
- **Root Certificate Authority (CA):** The top-level trusted entity
- **Intermediate CA:** Bridges trust between root and end-entity certificates
- **Leaf Certificate:** The certificate used by servers (e.g., websites)
- **Private Key:** Secret key used to sign or decrypt
- **Public Key:** Shared key used to verify signatures or encrypt data

---

### Flow
1. A server presents its certificate
2. The system checks the certificate chain:
   - Server → Intermediate → Root CA
3. The system verifies:
   - The certificate is valid (not expired)
   - The chain leads to a trusted root CA in the trust store
4. If all checks pass → connection is trusted  
5. If any check fails → connection is rejected

---

### Trust Implications
- Trust is not automatic — it is based on the presence of trusted root CAs
- If a root CA is trusted, everything it signs is trusted
- Removing a root CA breaks trust instantly
- Installing a malicious root CA can compromise all secure communications

---

## 4. Common Misconceptions

- **Misconception 1:** Encryption = trust  
  → Encryption protects data, but trust comes from certificate validation

- **Misconception 2:** If a certificate works, it is secure  
  → A certificate may appear valid if trust is manually provided (e.g., using `-CAfile`), even if the system does not actually trust it

---

## 5. Where This Shows Up

- **Web security:** HTTPS connections and browser trust warnings  
- **Internal enterprise systems:** Corporate root CAs and internal services  
- **Cloud environments:** Secure APIs, identity systems, and service communication  
- **DevOps workflows:** Code signing, CI/CD pipelines, and secure deployments  

---

## Mental Model

PKI is a system of **Identity + Trust + Verification**.

- **Identity:** Certificates prove who something claims to be  
- **Trust:** Root CAs define what is trusted  
- **Verification:** Systems validate certificate chains before allowing communication  

If any part fails, trust is broken.
