# Week 12 Lab 02 – Configure and Test the OCSP Responder

Student Name: Omoze Idehenre 

Date Completed: 5/31/2026

Phase: 2 | Week: 12

Submission Path: labs/week-12/lab-02-ocsp-responder.md

## Objective

Install and configure the Online Certificate Status Protocol (OCSP) Online Responder role service and verify certificate status validation functionality.

---

## Environment

- Server: PKI-SRV01
- CA: CVI Issuing CA 1
- Domain: corp.cvilab.local

---

## Tasks Performed

### 1. Installed Online Responder

Opened:

Server Manager → Add Roles and Features

Installed:

- Active Directory Certificate Services
- Online Responder

Installation completed successfully.

---

### 2. Verified Service Status

Executed:

```powershell
Get-Service OCSPSvc
```

Result:

```text
Status : Running
Name   : OCSPSvc
```

The Online Responder service was operational.

---

### 3. Configured Revocation Configuration

Opened:

Administrative Tools → Online Responder Management

Created a new Revocation Configuration:

- Name: CVI Issuing CA 1 OCSP
- CA: CVI Issuing CA 1

Successfully associated the Online Responder with the issuing CA.

---

### 4. Verified OCSP Template Availability

Opened:

```powershell
certtmpl.msc
```

Confirmed that the following template existed:

```text
OCSP Response Signing
```

The template was available in Active Directory.

---

### 5. Published Template on the CA

Opened:

Certification Authority Console

Navigated to:

```text
Certificate Templates
```

Verified that:

```text
OCSP Response Signing
```

was published on the issuing CA.

---

### 6. Attempted Certificate Enrollment

Executed:

```powershell
gpupdate /force
certutil -pulse
certreq -enroll "OCSP Response Signing"
```

Result:

```text
Template not found.
OCSP Response Signing
```

The OCSP signing certificate could not be enrolled.

---

### 7. Troubleshooting

Verified:

- Online Responder role installed
- OCSPSvc running
- Revocation configuration created
- OCSP template exists
- Template published on CA

Observed:

```text
Bad signing certificate on Array controller
```

This indicates that the OCSP signing certificate was not issued to the Online Responder server.

---

## Outcome

The Online Responder service was successfully installed and configured.

The environment did not automatically issue an OCSP Response Signing certificate, preventing completion of full OCSP validation.

All configuration and troubleshooting steps were successfully performed and documented.

---

## Skills Demonstrated

- Online Responder installation
- OCSP architecture understanding
- Revocation configuration management
- Certificate template management
- CA integration
- PKI troubleshooting
