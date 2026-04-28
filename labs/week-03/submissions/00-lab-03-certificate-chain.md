# Lab — Certificate Chain Analysis

## Overview
This lab focused on understanding how certificate chains establish trust in PKI systems. The goal was to identify the root, intermediate, and leaf certificates and analyze how they are connected through issuer and subject relationships.

---

## Certificate Chain Identification

1. Which certificate is the root CA?  
   The root CA is **USERTrust ECC Certification Authority**. It is trusted because it is a root certificate authority in the system trust store. While it may not appear self-signed in this output, it serves as the top-level trusted authority in the chain.

---

2. Which is the intermediate CA?  
   The intermediate CA is **Sectigo Public Server Authentication CA DV E36**. It is issued by a higher-level certificate authority and is responsible for signing the server certificate.

---

3. Which is the leaf certificate?  
   The leaf certificate is issued to **github.com** and is signed by the intermediate CA.

---

4. How does the Issuer field connect the chain?  
   The Issuer of the server certificate matches the Subject of the intermediate certificate, and the Issuer of the intermediate certificate connects upward toward the root certificate authority. This chaining establishes trust from the leaf certificate to the trusted root.

---

5. Why do intermediate certificates exist?  
   Intermediate certificates protect the root CA by allowing it to remain secure and offline while still enabling certificate issuance. This reduces risk while maintaining a scalable trust model.

---

## Certificate Roles

| Certificate        | Subject                                      | Issuer                                              | CA    |
|-------------------|---------------------------------------------|-----------------------------------------------------|-------|
| server.pem        | github.com                                  | Sectigo Public Server Authentication CA DV E36       | FALSE |
| intermediate.pem  | Sectigo Public Server Authentication CA DV E36 | Sectigo Public Server Authentication Root E46        | TRUE  |
| root.pem          | USERTrust ECC Certification Authority        | USERTrust ECC Certification Authority               | TRUE  |


---

## Key Findings

- Certificate chains establish trust by linking certificates through issuer and subject relationships.
- The leaf certificate represents the website identity.
- The intermediate certificate acts as a bridge between the leaf and the root.
- The root certificate is the trust anchor that systems rely on.

---

## Explanation

This matters because PKI systems rely on certificate chains to verify identity and establish secure communication. Browsers and systems trust root certificate authorities, and that trust is extended down the chain through intermediate certificates to the leaf certificate. Without this structure, secure HTTPS communication would not be possible.

---

## Challenges / Troubleshooting

- Initially assumed a DigiCert chain, but OpenSSL output revealed a Sectigo-based chain.
- Verified certificate roles by examining Subject and Issuer fields using OpenSSL commands.
- Ensured correct interpretation of the trust chain based on actual output rather than assumptions.

---

## Artifacts

- server.pem (leaf certificate)
- intermediate.pem (intermediate certificate)
- root.pem (root certificate)
- lab-03-certificate-chain.md (this write-up)

---

*CVI PKI Career Pathway — Foundations Phase*
