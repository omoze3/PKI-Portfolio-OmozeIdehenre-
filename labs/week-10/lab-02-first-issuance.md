# Lab 02: Issue Your First Certificate from a Custom Template

Student Name: Omoze Idehenre  
Date Completed: May 17, 2026  
Phase: 2 | Week: 10  

Submission Path: labs/week-10/lab-02-first-issuance.md

---

# Pre-Lab Verification

```powershell
Get-Service -Name CertSvc
```

```powershell
certutil -ping
```

CertSvc status: Running

CA responding (certutil -ping):

- [x] Yes

CVI-WebServer template visible in certtmpl.msc (from Lab 01):

- [x] Yes

---

# Part A — Publish the Template to the CA

Steps performed on PKI-SRV01:

- Opened certsrv.msc
- Expanded CVI Issuing CA 1
- Right-clicked Certificate Templates
- Selected New → Certificate Template to Issue
- Selected CVI-WebServer from the list
- Clicked OK

CVI-WebServer template now visible under Certificate Templates node:

- [x] Yes

## Screenshot or description of the Certificate Templates node showing CVI-WebServer:

The CVI-WebServer template appeared successfully under the Certificate Templates node within the Certification Authority console for CVI Issuing CA 1.

---

# Part B — Request the Certificate via MMC

Steps performed on PKI-SRV01, logged in as CORP\pki.admin:

- Opened mmc.exe
- Selected File → Add/Remove Snap-in
- Added Certificates snap-in

Selected:

- Computer account

- Navigated to Personal → Certificates
- Right-clicked Certificates → All Tasks → Request New Certificate
- Proceeded through the Certificate Enrollment wizard

## Certificate Enrollment wizard — enrollment policy selected:

Active Directory Enrollment Policy

## Templates shown in the wizard:

- Computer
- CVI-WebServer

CVI-WebServer template visible:

- [x] Yes

## Subject name entered (if prompted):

Subject:
- CN=cviissuingca1.corp.cvilab.local

Subject Alternative Name (DNS):
- cviissuingca1.corp.cvilab.local

Certificate request submitted:

- [x] Yes — certificate issued immediately

---

# Part C — Inspect the Issued Certificate

## General tab

| Field | Value |
|---|---|
| Issued to | cviissuingca1.corp.cvilab.local |
| Issued by | CVI Issuing CA 1 |
| Valid from | 5/17/2026 |
| Valid to | 4/25/2027 |

---

## Details tab — recorded fields

| Field | Value |
|---|---|
| Serial Number | 4400000003bf8819a444703a |
| Signature Algorithm | sha256RSA |
| Subject | CN=cviissuingca1.corp.cvilab.local |
| Key Usage | Digital Signature, Key Encipherment |
| Enhanced Key Usage | Server Authentication |
| Subject Alternative Name | DNS Name=cviissuingca1.corp.cvilab.local |
| Thumbprint | 697926d7522ad1d1d3b1e0e5b271579e37fa45f4 |

---

# Via certutil

```powershell
certutil -store My
```

## Full certutil output:

```powershell
================ Certificate 3 ================
Serial Number: 4400000003bf8819a444703a
Issuer: CN=CVI Issuing CA 1, DC=corp, DC=comp
 NotBefore: 5/17/2026 7:36 PM
 NotAfter: 4/25/2027 7:36 PM
Subject: CN=cviissuingca1.corp.cvilab.local
Certificate Template Name (Certificate Type): CVI-WebServer
Cert Hash(sha1): 697926d7522ad1d1d3b1e0e5b271579e37fa45f4
Key Container = te-3d72a1df-fffd-4671-8c26-42068bde06c3
Simple container name: te-3d72a1df-fffd-4671-8c26-42068bde06c3
Provider = Microsoft RSA SChannel Cryptographic Provider
Private key is NOT exportable
Encryption test passed
CertUtil: -store command completed successfully.
```

---

# In certsrv.msc — Issued Certificates Node

Does the certificate appear in the Issued Certificates node?

- [x] Yes

| Column | Value |
|---|---|
| Request ID | 3 |
| Requester Name | CORP\PKI-SRV01$ |
| Certificate Template | CVI-WebServer |
| Issued Common Name | cviissuingca1.corp.cvilab.local |
| Certificate Expiration Date | 4/25/2027 7:36 PM |

---

# Part D — Write-Up: The Issuance Workflow

When the CVI-WebServer template was published to the CA, Active Directory made the template available for enrollment requests through the Certification Authority service. Publishing the template allowed domain systems and authorized users to request certificates based on the template configuration and security permissions that were defined earlier in the lab.

During enrollment, the MMC Certificate Enrollment wizard generated a certificate signing request containing the subject name, public key, and template information. The CA evaluated the request against the template settings, enrollment permissions, and cryptographic requirements before issuing the certificate. After approval, the issued certificate was automatically placed in the Local Computer Personal certificate store because the request was performed using the Computer account context. The certificate chain successfully validated back to the CVI Root CA through the issuing CA.

One thing I did not expect was how many configuration dependencies exist between template permissions, CA publication, subject name configuration, and MMC enrollment behavior before a certificate can successfully issue.

---

# Submission Checklist

- [x] Pre-lab verification completed
- [x] Part A: CVI-WebServer template published to CVI Issuing CA 1
- [x] Part A: Template visible in certsrv.msc Certificate Templates node — confirmed
- [x] Part B: Certificate requested via MMC — request submitted
- [x] Part B: Enrollment wizard observations documented
- [x] Part C: Certificate details recorded from MMC (General + Details tabs)
- [x] Part C: certutil -store My output pasted
- [x] Part C: Certificate confirmed in certsrv.msc Issued Certificates node
- [x] Part D: Issuance workflow write-up completed in own words
- [x] File saved as lab-02-first-issuance.md
- [x] File committed to portfolio repo under labs/week-10/ 
