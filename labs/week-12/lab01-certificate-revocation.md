# Week 12 - Lab 01: Revoke a Certificate and Observe CRL Propagation

Student Name: Omoze Idehenre 
Date Completed: 5/31/2026
Phase: 2 | Week: 12
Submission Path: labs/week-12/lab-01-revoke-and-crl-propagation.md

## Objective

The purpose of this lab was to revoke an issued certificate, publish an updated Certificate Revocation List (CRL), and verify that the revoked certificate is no longer trusted within the PKI environment.

## Environment

* Root CA: CVI Root CA
* Issuing CA: CVI Issuing CA 1
* Domain: corp.cvilab.local
* Server: PKI-SRV01
* Certificate Template: CVI Code Signing

## Step 1 - Review Issued Certificates

Opened the Certification Authority console:

```powershell
certsrv.msc
```

Navigated to:

```text
Certification Authority
└── CVI Issuing CA 1
    └── Issued Certificates
```

Located the Code Signing certificate.

Certificate Details:

* Request ID: 8
* Requester: CORP\Administrator
* Certificate Template: CVI Code Signing
* Status: Issued

## Step 2 - Revoke Certificate

Within the Certification Authority console:

```text
Issued Certificates
→ Right-click Request ID 8
→ All Tasks
→ Revoke Certificate
```

Selected the revocation reason:

```text
Superseded
```

Verified the certificate moved into:

```text
Revoked Certificates
```

## Step 3 - Publish Updated CRL

Opened PowerShell and executed:

```powershell
certutil -CRL
```

Result:

```text
CertUtil: -CRL command completed successfully.
```

A new CRL was generated and published.

## Step 4 - Export Revoked Certificate

Created a temporary export directory:

```powershell
mkdir C:\Temp
```

Exported the revoked certificate as:

```text
C:\Temp\revoked.cer
```

Verified export:

```powershell
dir C:\Temp
```

Output:

```text
revoked.cer
```

## Step 5 - Inspect the CRL

Examined the published CRL:

```powershell
certutil -dump "C:\Windows\System32\CertSrv\CertEnroll\CVI Issuing CA 1.crl"
```

Observed:

* CRL successfully generated
* Signature Algorithm: SHA256RSA
* CRL hashes present
* Signature hashes present
* Next CRL publish date present

Example output:

```text
Next CRL Publish
Sunday, June 7, 2026
```

```text
Signature Algorithm:
sha256RSA
```

## Step 6 - Verify Revocation Status

Validated the exported certificate:

```powershell
certutil -verify C:\Temp\revoked.cer
```

Verification results:

```text
The certificate is revoked.
```

```text
Certificate is REVOKED
Leaf certificate is REVOKED (Reason=4)
```

Certificate information:

```text
Template: CVI Code Signing
```

```text
Serial Number:
44000000008df33d73e94d83c560000000000008
```

The validation process successfully detected that the certificate had been revoked and should no longer be trusted.

## Results

The certificate was successfully revoked and added to the Certificate Revocation List. After publishing a new CRL, certificate validation correctly detected the revocation and marked the certificate as untrusted.

Successful verification confirmed:

* Certificate revocation completed
* CRL publication completed
* Revoked certificate appeared in the CRL
* Certificate validation detected revocation
* PKI trust enforcement functioned correctly

## Key Concepts Learned

### Certificate Revocation

Certificate revocation invalidates a certificate before its expiration date and prevents further trusted use.

### Certificate Revocation Lists (CRLs)

CRLs are signed lists maintained by a Certificate Authority that contain revoked certificate serial numbers.

### Revocation Reasons

The revocation reason used in this lab was:

```text
Superseded
```

This indicates that a newer certificate has replaced the revoked certificate.

### Revocation Validation

Systems use CRLs during certificate validation to determine whether certificates remain trusted.

### PKI Lifecycle Management

Revocation is an important part of certificate lifecycle management and helps protect environments from compromised, replaced, or invalid certificates.

## Conclusion

In this lab, I successfully revoked a Code Signing certificate, generated a new CRL, exported the revoked certificate, and verified that the PKI environment correctly identified the certificate as revoked. This exercise demonstrated how revocation information is published and enforced throughout a Public Key Infrastructure environment.

