# Week 11 – Lab 03 – Compare Certificate Profiles Side by Side

**Student Name:** Omoze Idehenre  
**Date Completed:** 2026-05-24  
**Phase:** 2 | **Week:** 11  
**Submission Path:** labs/week-11/lab-03-profile-comparison.md

---

# Overview

Lab 03 is a synthesis exercise comparing three different certificate profiles issued across Weeks 10 and 11 of the CVI PKI Career Pathway labs.

The following certificates were analyzed:

1. TLS certificate issued from the `CVI-WebServer` template  
2. Service account certificate issued from the `CVI-ServiceAccount` template  
3. Code signing certificate issued from the `CVI-CodeSigning` template  

The purpose of this lab was to compare how certificate templates differ depending on their intended use case, including:

- Key Usage
- Extended Key Usage (EKU)
- Subject Name configuration
- Authentication purpose
- Trust relationships
- Security implications

This exercise demonstrates how enterprise PKI environments separate certificate purposes to reduce risk and enforce trust boundaries.

---

# Pre-Lab — Locate All Three Certificates

## Step 1 — Confirm Week 10 TLS Certificate

Command used:

```powershell
certutil -store My
```

### Week 10 TLS certificate present

- [x] Yes  
- [ ] No  

### TLS Thumbprint

```text
697926D7522AD1D1D3B1E0E5B271579E37FA45F4
```

---

# Step 2 — Record All Three Thumbprints

| Certificate | Template | Thumbprint |
|---|---|---|
| TLS Certificate | CVI-WebServer | 697926D7522AD1D1D3B1E0E5B271579E37FA45F4 |
| Service Account Certificate | CVI-ServiceAccount | 0F39D2B15181C1B7511D222A19B930B2A287B38C |
| Code Signing Certificate | CVI-CodeSigning | BD621E4E309E76534BA4D4A2EEE787268177C179 |

---

# Part A — Inspect All Three Certificates

---

# Certificate 1 — TLS (CVI-WebServer)

## Command Used

```powershell
certutil -store My 697926D7522AD1D1D3B1E0E5B271579E37FA45F4
```

## Full certutil Output

```text
Serial Number: 44000000003bf8819a444703a750000000000003
Issuer: CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local
NotBefore: 5/17/2026 7:23 AM
NotAfter: 4/25/2027 7:36 PM
Subject: CN=cviissuingca1.corp.cvilab.local

Template: CVI-WebServer

Cert Hash(sha1):
697926d7522ad1d1d3b1e0e5b271579e37fa45f4

Provider:
Microsoft RSA SChannel Cryptographic Provider

Private key is NOT exportable
Encryption test passed
```

---

# Certificate 2 — Service Account (CVI-ServiceAccount)

## Command Used

```powershell
Get-ChildItem Cert:\CurrentUser\My | Format-List Subject,Issuer,Thumbprint,NotBefore,SerialNumber,EnhancedKeyUsageList
```

## Full Output

```text
Subject:
CN=Administrator, CN=Users, DC=corp, DC=cvilab, DC=local

Issuer:
CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local

Thumbprint:
0F39D2B15181C1B7511D222A19B930B2A287B38C

NotBefore:
5/24/2026 5:52:44 AM

SerialNumber:
44000000006165D0A6376A728AE0000000000006

EnhancedKeyUsageList:
Client Authentication (1.3.6.1.5.5.7.3.2)
```

---

# Certificate 3 — Code Signing (CVI-CodeSigning)

## Command Used

```powershell
Get-ChildItem Cert:\CurrentUser\My | Format-List Subject,Issuer,Thumbprint,NotBefore,SerialNumber,EnhancedKeyUsageList
```

## Full Output

```text
Subject:
CN=Administrator, CN=Users, DC=corp, DC=cvilab, DC=local

Issuer:
CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local

Thumbprint:
BD621E4E309E76534BA4D4A2EEE787268177C179

NotBefore:
5/24/2026 7:01:32 PM

SerialNumber:
44000000008DF33D73E94D83C560000000000008

EnhancedKeyUsageList:
Code Signing (1.3.6.1.5.5.7.3.3)
```

---

# Comparison Table

| Field | TLS Certificate | Service Account Certificate | Code Signing Certificate |
|---|---|---|---|
| Template Name | CVI-WebServer | CVI-ServiceAccount | CVI-CodeSigning |
| Subject | CN=cviissuingca1.corp.cvilab.local | CN=Administrator, CN=Users, DC=corp, DC=cvilab, DC=local | CN=Administrator, CN=Users, DC=corp, DC=cvilab, DC=local |
| Subject Source | Supplied in request | Build from AD | Build from AD |
| Issuer | CVI Issuing CA 1 | CVI Issuing CA 1 | CVI Issuing CA 1 |
| Key Usage | Digital Signature, Key Encipherment | Digital Signature, Key Encipherment | Digital Signature |
| EKU | Server Authentication | Client Authentication | Code Signing |
| EKU OID(s) | 1.3.6.1.5.5.7.3.1 | 1.3.6.1.5.5.7.3.2 | 1.3.6.1.5.5.7.3.3 |
| Validity Period | 1 Year | 1 Year | 1 Year |
| Serial Number | 44000000003bf8819a444703a750000000000003 | 44000000006165D0A6376A728AE0000000000006 | 44000000008DF33D73E94D83C560000000000008 |
| Thumbprint | 697926D7522AD1D1D3B1E0E5B271579E37FA45F4 | 0F39D2B15181C1B7511D222A19B930B2A287B38C | BD621E4E309E76534BA4D4A2EEE787268177C179 |
| Request ID | 3 | 6 | 8 |

---

# Part B — Written Analysis

# 1 — Key Usage

Each certificate contains a different Key Usage configuration because each certificate supports a completely different cryptographic purpose.

The TLS certificate uses Digital Signature and Key Encipherment because HTTPS communication requires both authentication and encrypted session establishment. During a TLS handshake, the server proves its identity using a digital signature while also assisting in secure key exchange operations. Without Key Encipherment, the server would not be able to securely negotiate encrypted communication channels with clients.

The service account certificate also uses Digital Signature and Key Encipherment because client authentication certificates must securely authenticate the identity of the account to another system. The certificate proves identity while also enabling secure authentication exchanges between systems and services. These certificates are commonly used for automation accounts, APIs, scheduled tasks, and machine-to-machine trust relationships.

The code signing certificate primarily relies on Digital Signature because its purpose is not encryption or authentication to a remote server. Instead, the certificate validates integrity and publisher authenticity. When a PowerShell script or executable is signed, the operating system validates that the content has not been modified after signing and confirms that the signer is trusted.

---

# 2 — Extended Key Usage (EKU)

Each certificate profile contains a unique Extended Key Usage value because relying party systems enforce certificate usage restrictions based on intended purpose.

The TLS certificate uses the Server Authentication EKU. Web browsers, operating systems, and TLS clients check for this EKU during HTTPS validation. If the Server Authentication EKU were missing or incorrect, browsers would reject the certificate or display trust warnings because the certificate would not be authorized for web server authentication.

The service account certificate uses the Client Authentication EKU. Authentication services such as Active Directory, VPN systems, APIs, and enterprise applications validate this EKU when a client presents a certificate during login or service authentication. Without the Client Authentication EKU, the system would reject the certificate for identity verification purposes.

The code signing certificate uses the Code Signing EKU. PowerShell execution policy, Windows Defender Application Control, SmartScreen, and operating systems rely on this EKU to validate signed code. If a certificate lacking the Code Signing EKU attempted to sign scripts or executables, the operating system would reject the signature as invalid or untrusted.

---

# 3 — Subject Name Source

The TLS certificate uses "Supplied in request" because TLS certificates must match externally reachable DNS hostnames and server names. These hostnames are highly dynamic and may not exist as exact objects within Active Directory. For example, web applications, load balancers, APIs, reverse proxies, and internet-facing systems often require certificates for fully qualified domain names that administrators manually define during certificate enrollment.

In contrast, the service account and code signing certificates use "Build from Active Directory" because the certificates are directly tied to existing identity objects already stored within the domain. Since the certificates represent authenticated users or accounts, Active Directory can automatically populate subject names consistently and accurately from directory information.

---

# 4 — Security Question

A single certificate containing Server Authentication, Client Authentication, and Code Signing EKUs would create a severe security risk because compromise of one private key would allow an attacker to impersonate multiple trust roles simultaneously.

An attacker possessing the private key could impersonate a trusted HTTPS server, authenticate as a legitimate user or service account, and sign malicious PowerShell scripts or executables that appear trusted by enterprise systems. This would dramatically increase the blast radius of a single compromise because one certificate would bypass multiple security boundaries at once.

Separating EKUs into different certificate templates limits privilege scope and reduces the impact of certificate theft. Enterprise PKI environments intentionally separate these functions to enforce least privilege principles and compartmentalize trust relationships.

---

# Reflection

## Which certificate would be most critical to revoke quickly if compromised?

The code signing certificate would likely require the fastest revocation response because it could be used to sign malicious scripts, executables, or software updates that appear trusted across enterprise systems. A compromised code signing certificate could enable malware distribution, privilege escalation, persistence mechanisms, and trusted execution bypasses. The blast radius could impact many users and systems simultaneously.

---

## What would you add if presenting this to a security team?

If presenting this comparison to a security team, additional information would include:

- Certificate template permissions
- Enrollment agent restrictions
- Auto-enrollment configuration
- CRL and OCSP validation paths
- Private key storage provider details
- Key length and cryptographic algorithm comparison
- Exportability settings
- Certificate lifecycle management procedures
- Revocation testing evidence
- Logging and monitoring controls for certificate issuance

These details would help security teams evaluate the maturity, governance, and operational security posture of the PKI environment.

---

# Submission Checklist

- [x] Pre-lab thumbprints recorded
- [x] Part A completed
- [x] Comparison table completed
- [x] Key Usage analysis completed
- [x] EKU analysis completed
- [x] Subject Name source analysis completed
- [x] Security question completed
- [x] Reflection completed
- [x] File saved as lab-03-profile-comparison.md
- [x] File committed to portfolio repository
- [x] Request IDs recorded for Week 12
