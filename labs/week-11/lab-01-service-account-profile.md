# Lab 01 — Build a Certificate Profile for a Service Account

**Student Name:Omoze Idehenre   
**Date Completed:** May 24, 2026  
**Course:** CVI PKI Career Pathway — Phase 2  
**Week:** 11  
**Lab File:** `labs/week-11/lab-01-service-account-profile.md`

---

# Lab Objective

The purpose of this lab was to design, publish, enroll, validate, and export a certificate template specifically intended for service account authentication within an enterprise PKI environment using Microsoft Active Directory Certificate Services (AD CS).

This lab simulated how organizations securely replace password-based authentication with certificate-based authentication for applications, services, automation, and machine-to-machine identity verification.

---

# Environment Information

| Component | Value |
|---|---|
| Certificate Authority | CVI Issuing CA 1 |
| Root CA | CVI Root CA |
| Domain | corp.cvilab.local |
| Server | PKI-SRV01 |
| OS | Windows Server |
| Tools Used | certsrv.msc, certtmpl.msc, mmc.exe, certutil, PowerShell |
| Enrollment Policy | Active Directory Enrollment Policy |

---

# Pre-Lab Verification

## Verify CA Service

```powershell
Get-Service -Name CertSvc
```

### Result

The Active Directory Certificate Services service was running successfully.

---

## Verify CA Connectivity

```powershell
certutil -ping
```

### Result

The issuing CA responded successfully.

---

## Verify Active Directory Service Account

```powershell
Get-ADUser -Identity svc.autoenroll -Properties UserPrincipalName
```

### Result

The service account existed in Active Directory and was accessible for certificate enrollment.

---

# Part A — Certificate Template Design

# Open Certificate Templates Console

Opened:

```text
certtmpl.msc
```

---

# Duplicate Existing Template

## Source Template Selected

| Setting | Value |
|---|---|
| Base Template | User |

---

# Why the User Template Was Chosen

The User template already included identity-based authentication settings suitable for secure certificate enrollment. Since the service account required authentication capabilities and Active Directory integration, the User template provided the closest baseline while still allowing customization.

---

# Template General Settings

| Field | Value |
|---|---|
| Template Display Name | CVI Service Account |
| Template Internal Name | CVI-ServiceAccount |
| Template Version | Version 2 |

---

# Compatibility Settings

| Setting | Value |
|---|---|
| Certification Authority Compatibility | Windows Server 2016 |
| Certificate Recipient Compatibility | Windows 10 / Server 2016 |

---

# Key Usage Configuration

| Key Usage | Enabled | Reason |
|---|---|---|
| Digital Signature | Yes | Required for authentication and identity validation |
| Key Encipherment | Yes | Required for TLS and secure session negotiation |
| Data Encipherment | No | Not needed for this service account |
| Non-Repudiation | No | Not intended for legal signing |

---

# Explanation of Key Usage Decisions

Digital Signature allows the certificate holder to prove identity cryptographically during authentication.

Key Encipherment allows secure encryption and TLS key exchange operations.

Data Encipherment was excluded because the service account was not intended for file or bulk data encryption.

Non-Repudiation was excluded because the certificate was not being used for legally binding document signing.

---

# Extended Key Usage (EKU) Configuration

| EKU | Enabled | OID | Reason |
|---|---|---|---|
| Client Authentication | Yes | 1.3.6.1.5.5.7.3.2 | Required for service authentication |
| Server Authentication | No | 1.3.6.1.5.5.7.3.1 | Service account is not hosting HTTPS |
| Code Signing | No | 1.3.6.1.5.5.7.3.3 | Not used for software publishing |
| Secure Email | No | 1.3.6.1.5.5.7.3.4 | Not used for email encryption |

---

# Explanation of EKU Decisions

Only Client Authentication was enabled because the service account certificate was intended solely for identity validation and secure authentication.

Additional EKUs were intentionally excluded to minimize certificate misuse and reduce attack surface.

---

# Subject Name Configuration

| Setting | Value |
|---|---|
| Subject Name Format | Build from Active Directory |
| Identity Included | UPN |

---

# Explanation of Subject Name Decision

Using Active Directory integration ensures that certificate identities remain centralized, standardized, and automatically populated from trusted directory information.

This reduces manual configuration errors and improves enterprise identity consistency.

---

# Validity Period Configuration

| Setting | Value |
|---|---|
| Validity Period | 1 Year |
| Renewal Period | 6 Weeks |

---

# Explanation of Validity Decision

A shorter validity period improves security by reducing long-term exposure if credentials are compromised.

The renewal period allows administrators time to rotate or renew certificates before expiration occurs.

---

# Security Permissions

| Account / Group | Read | Enroll | Autoenroll |
|---|---|---|---|
| Authenticated Users | Yes | No | No |
| CORP\svc.autoenroll | Yes | Yes | Yes |
| Domain Computers | Yes | No | No |

---

# Explanation of Security Decisions

The template was restricted so only the intended service account could enroll and autoenroll certificates.

This reduces the risk of unauthorized certificate issuance.

---

# Save Template

After configuration was completed:

- Clicked Apply
- Clicked OK

The template successfully appeared inside:

```text
certtmpl.msc
```

---

# Part B — Publish Template to Certificate Authority

# Open Certification Authority Console

Opened:

```text
certsrv.msc
```

---

# Publish Template

Steps performed:

1. Expanded:
   - CVI Issuing CA 1

2. Right-clicked:
   - Certificate Templates

3. Selected:
   - New → Certificate Template to Issue

4. Selected:
   - CVI-ServiceAccount

5. Clicked:
   - OK

---

# Result

The template successfully appeared under:

```text
CVI Issuing CA 1 → Certificate Templates
```

---

# Configure AIA Extension

Opened:

```text
CVI Issuing CA 1 → Properties → Extensions
```

Selected Extension:

```text
Authority Information Access (AIA)
```

Configured HTTP location:

```text
http://pki.cvilab.local/CertEnroll/<CaName><CertificateName>.crt
```

Enabled:

- [x] Include in the AIA extension of issued certificates
- [x] Include in the online certificate status protocol (OCSP) extension

---

# Why AIA Matters

AIA allows clients and systems to locate issuing CA certificates and certificate validation information during chain building and trust validation operations.

Without AIA configuration, certificate chains may fail to validate properly.

---

# Part C — Request Certificate

# Open MMC Certificates Console

Opened:

```text
mmc.exe
```

Added Snap-In:

```text
Certificates → Current User
```

Navigated to:

```text
Personal → Certificates
```

---

# Start Enrollment Wizard

Right-clicked:

```text
All Tasks → Request New Certificate
```

---

# Enrollment Policy

Selected:

```text
Active Directory Enrollment Policy
```

---

# Templates Displayed

The following templates appeared:

- Administrator
- Basic EFS
- CVI Service Account
- EFS Recovery Agent
- User

---

# Certificate Requested

Selected:

```text
CVI Service Account
```

Clicked:

```text
Enroll
```

---

# Enrollment Result

| Status | Result |
|---|---|
| Enrollment | Successful |
| Certificate Installed | Yes |

---

# Part D — Verify Certificate

# Verify in MMC

Navigated to:

```text
Certificates → Current User → Personal → Certificates
```

Observed issued certificates.

Two Administrator certificates appeared in the store after enrollment.

---

# Verify in Certification Authority Console

Navigated to:

```text
CVI Issuing CA 1 → Issued Certificates
```

Observed issued certificates.

---

# Issued Certificates Observed

| Request ID | Requester Name | Template | Status |
|---|---|---|---|
| 6 | CORP\Administrator | CVI Service Account | Issued |
| 7 | CORP\Administrator | CVI Service Account | Issued |

---

# PowerShell Verification Using certutil

Executed:

```powershell
certutil -store My
```

---

# Certificate Details Captured

## Certificate 1

| Field | Value |
|---|---|
| Issuer | CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local |
| Subject | CN=cviissuingca1.corp.cvilab.local |
| Template | CVI-WebServer |
| Cert Hash (SHA1) | 697926d7522ad1d1d3b1e0e5b271579e37fa45f4 |
| Provider | Microsoft RSA SChannel Cryptographic Provider |
| Private Key Exportable | No |
| Encryption Test | Passed |

---

## Certificate 2

| Field | Value |
|---|---|
| Serial Number | 5800000002f7714edc7f317c46000000000002 |
| Issuer | CN=CVI Root CA, DC=corp, DC=cvilab, DC=local |
| Subject | CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local |
| Template | SubCA |
| Cert Hash (SHA1) | 5137a597de2c3085ec5816c7f11edc18cfcdbaf8 |
| Provider | Microsoft Software Key Storage Provider |
| Signature Test | Passed |

---

# Export Certificate

Navigated to:

```text
Personal → Certificates
```

Right-clicked certificate:

```text
All Tasks → Export
```

---

# Export Settings

| Setting | Value |
|---|---|
| Export Format | PKCS #12 (.PFX) |
| Include Certificate Path | Yes |
| Enable Certificate Privacy | Yes |
| Password Protection | Enabled |
| Encryption | TripleDES-SHA1 |

---

# Export Location

Saved to:

```text
C:\Users\Administrator\Desktop\admin-cert.pfx
```

---

# Why PFX Was Used

The PFX format allows both the certificate and associated private key to be exported together securely.

This format is commonly used for:

- Service migrations
- Backup operations
- TLS deployments
- Application authentication

---

# Security Observation

One observed certificate stated:

```text
Private key is NOT exportable
```

This is an important enterprise security control because non-exportable keys reduce the risk of credential theft and unauthorized duplication.

---

# Risks of Password-Based Service Accounts

Password-based service accounts introduce several risks:

- Password reuse
- Weak passwords
- Expired passwords causing outages
- Credential dumping attacks
- Human error during rotation
- Long-lived static credentials

Certificate-based authentication improves security by replacing reusable secrets with cryptographic identity validation.

---

# Why Service Account Certificates Matter

Service account certificates provide:

- Strong authentication
- Automated trust validation
- Reduced password exposure
- Better auditability
- Reduced attack surface
- Scalable enterprise identity management

Organizations commonly use these certificates for:

- IIS
- APIs
- Automation platforms
- Scheduled tasks
- Machine authentication
- Internal services

---

# Reflection

One of the most important lessons learned during this lab was understanding how certificate templates enforce security boundaries inside enterprise PKI environments.

The lab demonstrated that certificate issuance is not simply about generating certificates, but about designing controlled trust relationships using:

- Template restrictions
- EKU limitations
- Enrollment permissions
- Identity mapping
- AIA configuration
- Certificate lifecycle management

A non-obvious realization was how critical proper template scoping is. Even a small mistake in EKU configuration or enrollment permissions could allow unauthorized identities to obtain highly privileged certificates.

---

# Screenshots Collected

- MMC Certificates Snap-In
- Certificate Enrollment Wizard
- Certificate Installation Success
- Issued Certificates Console
- AIA Configuration
- certutil PowerShell Output
- Export Wizard
- Exported PFX File

---

# Final Result

## Successfully Completed

- [x] Created CVI-ServiceAccount template
- [x] Published template to issuing CA
- [x] Configured AIA extension
- [x] Requested certificate
- [x] Successfully enrolled certificate
- [x] Verified issued certificate
- [x] Verified via certutil
- [x] Exported certificate as PFX
- [x] Observed issued certificates in certsrv.msc
- [x] Completed certificate lifecycle validation

---
