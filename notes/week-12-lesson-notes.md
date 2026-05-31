# Week 12 Lesson Notes — Certificate Revocation and Validation

## 1. Core Concept

Certificate revocation is the process of invalidating a digital certificate before its expiration date. Revocation is necessary when a certificate has been compromised, issued incorrectly, or is no longer trusted.

Public Key Infrastructure (PKI) relies on more than certificate issuance. Organizations must also be able to determine whether a certificate remains trustworthy after issuance. This is accomplished through Certificate Revocation Lists (CRLs) and the Online Certificate Status Protocol (OCSP).

CRLs provide a published list of revoked certificates, while OCSP provides real-time certificate status information through an Online Responder.

---

## 2. Why It Matters

In enterprise environments, certificates are used for:

* Web servers
* VPN authentication
* Smart cards
* Code signing
* Internal applications
* Device authentication

If a private key is stolen or a certificate is compromised, organizations must immediately revoke trust in that certificate.

Without revocation mechanisms, users and systems could continue trusting malicious certificates, creating significant security risks.

Certificate revocation helps maintain trust throughout the certificate lifecycle.

---

## 3. Technical Breakdown

### Definition

Certificate revocation is the process of marking a certificate as no longer valid before its expiration date.

### Components

#### Certificate Authority (CA)

Issues certificates and maintains revocation information.

#### Certificate Revocation List (CRL)

A signed file published by a CA that contains revoked certificate serial numbers.

#### Delta CRL

A smaller CRL containing only changes since the last full CRL publication.

#### CRL Distribution Point (CDP)

The location where clients retrieve CRLs.

#### Online Certificate Status Protocol (OCSP)

A protocol used to provide near real-time certificate status responses.

#### Online Responder

A Microsoft service that answers OCSP requests on behalf of a CA.

---

### Flow

1. A certificate is issued.
2. A certificate becomes compromised or invalid.
3. The CA revokes the certificate.
4. Revocation information is published.
5. Clients validate certificate status using:

   * CRL retrieval
   * OCSP requests
6. The client determines whether the certificate is trusted or revoked.

---

### Trust Implications

Trust does not end when a certificate is issued.

Organizations must continuously validate:

* Certificate validity
* Certificate expiration
* Certificate revocation status
* Certificate chain integrity

Failure to maintain revocation information can allow compromised certificates to remain trusted.

---

## 4. Common Misconceptions

### Misconception 1

"A valid certificate is always trusted."

Reality:

A certificate can still be within its validity period but may have been revoked due to compromise or misuse.

---

### Misconception 2

"OCSP replaces CRLs."

Reality:

OCSP supplements CRLs. Most PKI environments maintain both revocation mechanisms to ensure availability and reliability.

---

## 5. Where This Shows Up

### Web Security

Web browsers validate certificates and often perform revocation checking using OCSP or CRLs.

Examples:

* HTTPS websites
* E-commerce platforms
* Banking applications

---

### Internal Enterprise Systems

Organizations use revocation checking for:

* Domain authentication
* Smart card logon
* Internal web portals
* Secure email

---

### Cloud Environments

Cloud platforms use certificates extensively for:

* API authentication
* Service identity
* Mutual TLS
* Secure communications

Examples:

* AWS
* Azure
* Google Cloud

---

### DevOps Workflows

Certificates are used to secure:

* CI/CD pipelines
* Code signing processes
* Kubernetes clusters
* Container registries

Revocation ensures compromised credentials cannot continue being trusted.

---

## Mental Model

Identity answers:

"Who are you?"

Trust answers:

"Should I trust you?"

Verification answers:

"Can I prove you are still trustworthy right now?"

Certificate issuance establishes identity.

Certificate chains establish trust.

CRLs and OCSP provide ongoing verification.

A secure PKI environment depends on all three:

**Identity + Trust + Verification**

