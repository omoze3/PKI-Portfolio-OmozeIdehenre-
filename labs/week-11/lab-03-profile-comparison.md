# Week 11 – Lab 02 – Code Signing Certificate Validation

## Objective

The objective of this lab was to create, configure, publish, enroll, and validate a Code Signing certificate template within an Active Directory Certificate Services (AD CS) environment. This lab demonstrates how enterprise PKI infrastructures establish authenticity, integrity, and trust for PowerShell scripts and administrative tooling.

---

# Environment Information

| Component | Value |
|---|---|
| Virtual Machine | PKI-SRV01 |
| Domain | corp.cvilab.local |
| Enterprise CA | CVI Issuing CA 1 |
| Root CA | CVI Root CA |
| Operating System | Windows Server 2022 |
| Tools Used | PowerShell, certutil, certtmpl.msc, certsrv.msc, certmgr.msc |
| PKI Infrastructure | Active Directory Certificate Services (AD CS) |

---

# Lab Goals

- Validate Active Directory Certificate Services
- Create a custom Code Signing certificate template
- Configure cryptographic settings
- Configure enhanced key usage policies
- Publish the template to the Enterprise CA
- Enroll a Code Signing certificate
- Create a PowerShell test script
- Digitally sign the script
- Validate Authenticode signature trust

---

# Step 1 – Validate Active Directory Certificate Services

## PowerShell Command

```powershell
Get-Service -Name CertSvc
```

## Result

```text
Status   Name      DisplayName
------   ----      -----------
Running  CertSvc   Active Directory Certificate Services
```

## Validation

Confirmed that Active Directory Certificate Services was operational and running successfully.

---

# Step 2 – Validate CA Connectivity

## PowerShell Command

```powershell
certutil -ping
```

## Result

```text
Connecting to PKI-SRV01.corp.cvilab.local\CVI Issuing CA 1 ...
Server "CVI Issuing CA 1" ICertRequest2 interface is alive (94ms)
CertUtil: -ping command completed successfully.
```

## Validation

Successfully validated communication with the Enterprise Issuing CA.

---

# Step 3 – Review Existing Certificate Information

## PowerShell Output Observed

```text
Issuer: CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local

Subject: CN=cviissuingca1.corp.cvilab.local

Template: CVI-WebServer

Provider = Microsoft RSA SChannel Cryptographic Provider

Private key is NOT exportable

Encryption test passed
```

---

# Step 4 – Review Intermediate CA Information

## PowerShell Output Observed

```text
Certificate Template Name (Certificate Type): SubCA

Template: SubCA, Subordinate Certification Authority

Provider = Microsoft Software Key Storage Provider

Signature test passed

CertUtil: -store command completed successfully.
```

## Validation

Confirmed the subordinate CA certificate chain and signing functionality were operating properly.

---

# Step 5 – Open Certificate Templates Console

## Run Command

```text
certtmpl.msc
```

## Actions Performed

- Opened Certificate Templates Console
- Located built-in Code Signing template
- Duplicated the default template for customization

---

# Step 6 – Configure Compatibility Settings

## Compatibility Configuration

| Setting | Value |
|---|---|
| Certification Authority | Windows Server 2003 |
| Certificate Recipient | Windows XP / Server 2003 |

## Resulting Changes

```text
Do not store certificates and requests in the CA database
Do not include revocation information in issued certificates
```

---

# Step 7 – Configure General Template Settings

## General Configuration

| Setting | Value |
|---|---|
| Template Display Name | CVI Code Signing |
| Template Name | CVICodeSigning |
| Validity Period | 1 Year |
| Renewal Period | 6 Weeks |

---

# Step 8 – Configure Request Handling

## Request Handling Settings

| Setting | Value |
|---|---|
| Purpose | Signature and Encryption |
| Allow Private Key Export | Enabled |
| Enrollment Method | Enroll subject without requiring user input |

## Security Notes

Enabled private key exportability for lab testing and validation purposes.

---

# Step 9 – Configure Cryptography Settings

## Cryptography Configuration

| Setting | Value |
|---|---|
| Provider Category | Key Storage Provider |
| Algorithm Name | RSA |
| Minimum Key Size | 4096 |
| Request Hash | SHA1 |

## Providers

```text
Microsoft Software Key Storage Provider
```

## Security Significance

4096-bit RSA keys provide stronger cryptographic protection for enterprise code signing operations.

---

# Step 10 – Configure Application Policies

## Application Policy Configuration

| Policy | Value |
|---|---|
| Enhanced Key Usage | Code Signing |
| Critical Extension | Enabled |

## Validation

The certificate was restricted specifically for Code Signing usage.

---

# Step 11 – Publish Certificate Template

## Run Command

```text
certsrv.msc
```

## Actions Performed

- Opened Certification Authority Console
- Expanded:

```text
CVI Issuing CA 1
```

- Navigated to:

```text
Certificate Templates
```

- Selected:

```text
New → Certificate Template to Issue
```

- Published:

```text
CVI Code Signing
```

---

# Step 12 – Enroll for Code Signing Certificate

## Run Command

```text
certmgr.msc
```

## Enrollment Location

```text
Certificates - Current User → Personal → Certificates
```

## Actions Performed

- Requested new certificate
- Selected:
  
```text
CVI Code Signing
```

- Completed enrollment wizard successfully

---

# Step 13 – Validate Certificate Enrollment

## Validation Console

```text
Certificates - Current User\Personal\Certificates
```

## Result

Observed enrolled Code Signing certificate:

| Property | Value |
|---|---|
| Issued To | Administrator |
| Issued By | CVI Issuing CA 1 |
| Intended Purpose | Code Signing |
| Expiration Date | 4/25/2027 |

---

# Step 14 – Create Script Directory

## PowerShell Command

```powershell
New-Item -Path "C:\Scripts" -ItemType Directory -Force
```

## Result

```text
Directory: C:\

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         5/24/2026   7:18 PM                Scripts
```

---

# Step 15 – Create PowerShell Test Script

## PowerShell Commands

```powershell
$scriptContent = @'
# CVI Phase 2 - Week 11 Code Signing Test
Write-Host "This script is signed with a CVI code signing certificate."
Write-Host "Issued to: Administrator"
Write-Host "Date: $(Get-Date)"
'@
```

```powershell
Set-Content -Path "C:\Scripts\Test-CVI.ps1" -Value $scriptContent
```

---

# Step 16 – Validate Script Creation

## PowerShell Command

```powershell
Get-ChildItem C:\Scripts
```

## Result

```text
Directory: C:\Scripts

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         5/24/2026   7:32 PM            184 Test-CVI.ps1
```

---

# Step 17 – Retrieve Code Signing Certificate

## PowerShell Command

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object {$_.EnhancedKeyUsageList.FriendlyName -contains "Code Signing"} | Select-Object -First 1
```

## Validate Certificate Properties

```powershell
$cert | Select-Object Subject, Thumbprint, NotAfter
```

## Result

```text
Subject:
CN=Administrator, CN=Users, DC=corp, DC=cvilab, DC=local

Thumbprint:
BD621E4E309E76534BA4D4A2EEE787268177C179

NotAfter:
4/25/2027 7:36 PM
```

---

# Step 18 – Digitally Sign PowerShell Script

## PowerShell Command

```powershell
Set-AuthenticodeSignature "C:\Scripts\Test-CVI.ps1" $cert
```

## Result

```text
Directory: C:\Scripts

SignerCertificate                         Status   Path
-----------------                         ------   ----
BD621E4E309E76534BA4D4A2EEE787268177C179 Valid    Test-CVI.ps1
```

---

# Step 19 – Validate Authenticode Signature

## PowerShell Command

```powershell
Get-AuthenticodeSignature "C:\Scripts\Test-CVI.ps1"
```

## Result

```text
Directory: C:\Scripts

SignerCertificate                         Status   Path
-----------------                         ------   ----
BD621E4E309E76534BA4D4A2EEE787268177C179 Valid    Test-CVI.ps1
```

---

# Final Validation

## Successful Outcome

```text
Status : Valid
```

The PowerShell script was successfully signed using the enterprise-issued Code Signing certificate.

---

# Security Significance

This lab demonstrates how enterprise PKI environments secure administrative tooling and automation workflows using digital signatures.

Code signing provides:

- Script authenticity
- Integrity validation
- Tamper protection
- Trust verification
- Enterprise software assurance
- Secure administrative automation

---

# Enterprise Use Cases

Code Signing certificates are commonly used for:

- PowerShell automation
- Secure DevOps pipelines
- Enterprise application publishing
- Driver signing
- Secure software distribution
- Administrative scripting environments
- CI/CD security workflows

---

# Key PKI Concepts Learned

- Enterprise Certificate Authorities
- Certificate Templates
- Enhanced Key Usage (EKU)
- RSA Cryptography
- Authenticode Signatures
- PowerShell Script Signing
- Certificate Enrollment
- Trust Chains
- Digital Signature Validation

---

# Lab Outcome Summary

Successfully completed:

- Enterprise CA validation
- Template duplication and customization
- Cryptographic configuration
- Enhanced Key Usage configuration
- Template publication
- Certificate enrollment
- Script creation
- Digital script signing
- Signature verification

Final Result:

```text
Authenticode Signature Status: VALID
```
