# Lab 02: AD CS Console Exploration & CA Hierarchy Documentation

**Student Name:** Omoze Idehenre  
**Date Completed:** May 9, 2026  
**Phase:** 2 | **Week:** 9  

**Submission Path:** `labs/week-09/lab-02-environment-documentation.md`

---

# Part A — AD CS Console Exploration (PKI-SRV01)

## CA Console Nodes — Observations

| Node | Contents / Observations |
|---|---|
| Revoked Certificates | Stores certificates that have been revoked before expiration and are no longer trusted. |
| Issued Certificates | Displays certificates successfully issued by the Certification Authority. |
| Pending Requests | Stores certificate requests awaiting approval or processing. |
| Certificate Templates | Displays templates available for certificate enrollment and issuance. |

---

# CA Properties — Key Settings

## General Tab

**CA Name:** `CVI Issuing CA 1`  
**Computer Name:** `PKI-SRV01`

---
Additional observations:
- Provider: Microsoft Software Key Storage Provider
- Hash Algorithm: SHA256

---

## Extensions Tab — CRL Distribution Points (CDP)

Observed CDP publication paths included:

- `C:\Windows\System32\CertSrv\CertEnroll\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`
- `ldap:///CN=<CATruncatedName><CRLNameSuffix>,CN=<ServerShortName>`

These locations are used to publish and distribute Certificate Revocation Lists (CRLs) for certificate validation and revocation checking.

---

## Extensions Tab — Authority Information Access (AIA)

Observed AIA publication paths included:

- `C:\Windows\System32\CertSrv\CertEnroll\<ServerDNSName>_<CaName>`
- `ldap:///CN=<CATruncatedName>,CN=AIA,CN=Public Key Services`

These locations are used to publish and retrieve Certification Authority certificate information for chain validation.

---

## Storage Tab

Database Path:
`C:\Windows\system32\CertLog`

Log Path:
`C:\Windows\system32\CertLog`

---

# Certificate Templates Console (certtmpl.msc)

## Templates visible in the forest (list what you observed)

| Template Name | Intended Purpose |
|---|---|
| Directory Email Replication | Directory Service Email Replication |
| Domain Controller Authentication | Client Authentication, Server Authentication |
| Kerberos Authentication | Client Authentication, Server Authentication |
| EFS Recovery Agent | File Recovery |
| Basic EFS | Encrypting File System |
| Domain Controller | Client Authentication, Server Authentication |
| Web Server | Server Authentication |
| Computer | Client Authentication, Server Authentication |
| User | Encrypting File System, Secure Email |
| Subordinate Certification Authority | All Purposes |
| Administrator | Microsoft Trust List Signing, Encrypting File System |


---

# Part B — CA Hierarchy Verification (PKI-SRV01)

## Command: certutil -store -enterprise Root

```powershell
Root "Trusted Root Certification Authorities"

================ Certificate 0 ================
Serial Number: 26373e51a6ab660340c47caef2232ce1
Issuer: CN=CVI Root CA, DC=corp, DC=cvilab, DC=local
NotBefore: 4/25/2026 6:15 PM
NotAfter: 4/25/2046 6:25 PM
Subject: CN=CVI Root CA, DC=corp, DC=cvilab, DC=local
CA Version: V0.0
Signature matches Public Key
Root Certificate: Subject matches Issuer
Cert Hash(sha1): b805e6ab548f6e7c57d3989f61de7fe6a51031d1

No key provider information
Cannot find the certificate and private key for decryption.

CertUtil: -store command completed successfully.
```

## What did you see? (Subject, Issuer, Thumbprint — describe in your own words)

The enterprise Root certificate store contained the CVI Root CA certificate. The Subject and Issuer matched, confirming it is a self-signed Root CA certificate and the trust anchor for the environment. The certificate hash (thumbprint) uniquely identified the Root CA certificate, and the validity period extended to 2046, reflecting the long lifecycle typically used for offline Root CAs.

---

## Command: certutil -store -enterprise CA

```powershell
CA "Intermediate Certification Authorities"

================ Certificate 0 ================
Serial Number: 5800000002f7714edc7f317c46000000000002
Issuer: CN=CVI Root CA, DC=corp, DC=cvilab, DC=local
NotBefore: 4/25/2026 7:26 PM
NotAfter: 4/25/2027 7:36 PM
Subject: CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local
CA Version: V0.0
Certificate Template Name (Certificate Type): SubCA
Non-root Certificate
Template: SubCA, Subordinate Certification Authority
Cert Hash(sha1): 5137a597de2c3085ec5816c7f11edc18cfcdbaf8

No key provider information

Provider = Microsoft Software Key Storage Provider
Simple container name: CVI Issuing CA 1

Unique container name:
b52f658bb3f263e6f529f3a0187c63bc_f0a99c17-76d3-498a-97de-2992c06105fd

ERROR: missing key association property: CERT_KEY_IDENTIFIER_PROP_ID

Signature test passed

CertUtil: -store command completed successfully.
```

## What did you see? (Subject, Issuer, Thumbprint — describe in your own words)

The enterprise CA store contained the CVI Issuing CA 1 certificate, which was issued by the CVI Root CA. Unlike the Root CA certificate, this certificate was a subordinate/intermediate CA certificate used for operational certificate issuance within the environment. The certificate template type was listed as SubCA, confirming its role as a subordinate Certification Authority. The SHA1 certificate hash uniquely identified the issuing CA certificate, and the signature validation completed successfully.

# Part C — Active Directory Structure (DC01)

## Active Directory Users and Computers (dsa.msc)

### PKI Admins OU — accounts found

The following accounts and groups were present in the PKI Admins OU:

- Cert Manager
- PKI Admin
- PKI Admins (Security Group)
### pki.admin account — group memberships

The PKI Admin account was a member of the following groups:

- Domain Admins
- Domain Users
- PKI Admins

### cert.manager account — group memberships

The Cert Manager account was a member of the following groups:

- Domain Users
- PKI Admins

### Domain-joined computer accounts found

The following domain-joined computer accounts were observed:

- PKI-SRV01

Additional observations:
- The CVI Servers OU was present but currently empty.

## Active Directory Sites and Services (dssite.msc)

### Server registered under Default-First-Site-Name

- DC01

## Certificate Templates Console (certtmpl.msc on DC01)

Did templates appear here, confirming they are stored in AD?

- [x] Yes

Certificate template functionality was accessible through the domain environment, confirming that certificate template objects are integrated with Active Directory.

---

# Part D — Environment Summary Write-Up

## 1. Environment Topology

The environment consisted of three virtual machines:

- DC01 — Domain Controller hosting Active Directory Domain Services and DNS
- PKI-SRV01 — Enterprise Issuing Certification Authority responsible for certificate issuance and management
- Root-CA — Offline standalone Root Certification Authority used as the trust anchor for the PKI hierarchy

The environment used internal networking with PKI-SRV01 successfully communicating with DC01 during connectivity testing.

---

## 2. CA Hierarchy

The PKI hierarchy consisted of an offline Root CA and an online Enterprise Issuing CA. The Root CA was self-signed and acted as the trust anchor for the environment. The CVI Issuing CA 1 certificate was issued by the CVI Root CA using the Subordinate Certification Authority template.

The Root CA is intentionally kept offline to reduce security exposure and protect the highest level signing authority in the PKI environment.

---

## 3. Certificate Templates

Several certificate templates were published to CVI Issuing CA 1, including:

- Web Server
- Computer
- User
- Domain Controller Authentication
- Kerberos Authentication
- Subordinate Certification Authority

The Web Server template is used for server authentication certificates, while the User template supports user authentication, encryption, and secure email functionality.

---

## 4. Active Directory Structure

The Active Directory environment included a dedicated PKI Admins organizational unit containing PKI administrative accounts and groups.

The PKI Admin account had elevated privileges through Domain Admins membership and was used for infrastructure administration. The Cert Manager account had more limited permissions focused on certificate management operations.

The environment also contained domain-joined systems such as PKI-SRV01 and Active Directory site configuration through Default-First-Site-Name.

---

## 5. One Thing I Found Interesting or Unexpected

One interesting observation was how closely Active Directory integrates with Certificate Services. Certificate templates, CA publication paths, CRLs, and authentication services were all connected through Active Directory infrastructure and replication.

