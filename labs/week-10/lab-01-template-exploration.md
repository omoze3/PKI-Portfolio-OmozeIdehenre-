# Lab 01: Explore and Duplicate a Certificate Template

Student Name: Omoze Idehenre  
Date Completed: May 17, 2026  
Phase: 2 | Week: 10  

Submission Path: labs/week-10/lab-01-template-exploration.md

---

# Pre-Lab Verification

```powershell
Get-Service -Name CertSvc
```

```powershell
certutil -ping
```

```powershell
certutil -store -enterprise CA
```

All checks passed:

- [x] Yes

---

# Part A — Explore Three Built-in Templates

# Template 1: User

| Field | Value |
|---|---|
| Template Display Name | User |
| Template Name (internal) | User |
| Minimum Supported CA | Windows 2000 |
| Validity Period | 1 year |
| Renewal Period | 6 weeks |

## General tab — notes:

The User template is designed for user identity certificates and supports certificate publication in Active Directory. It is intended for user authentication, encryption, and secure communications.

## Request Handling tab — Purpose:

Signature and Encryption

The User template is configured to support both signing and encryption operations for user identities and secure communications.

## Subject Name tab — Subject name format:

Built from information in Active Directory

Included fields:
- E-mail name

The User template automatically builds the subject identity from Active Directory user account information rather than requiring the subject name to be manually supplied in the request.

## Extensions tab — Key Usage:

- Digital Signature
- Key Encipherment

The Key Usage extension is marked as a critical extension.
## Extensions tab — Application Policies (EKU):

- Encrypting File System
- Secure Email
- Client Authentication
# Template 2: Computer

| Field | Value |
|---|---|
| Template Display Name | Computer |
| Template Name (internal) | Machine |
| Validity Period | 1 year |
| Renewal Period | 6 weeks |

## Request Handling tab — Purpose:

Signature and encryption

## Subject Name tab:

Built from information in Active Directory.

Type of subject:
- Computer or other device

## Extensions tab — Key Usage:

- Digital Signature
- Key Encipherment

The Key Usage extension is marked as critical.

## Extensions tab — Application Policies (EKU):

- Client Authentication
- Server Authentication
# Template 3: Web Server

| Field | Value |
|---|---|
| Template Display Name | Web Server |
| Template Name (internal) | WebServer |
| Validity Period | 2 years |
| Renewal Period | 6 weeks |

## Request Handling tab — Purpose:

Signature and encryption

## Subject Name tab:

Supplied in the request.

Type of subject:
- Computer or other device

## Extensions tab — Key Usage:

- Digital Signature
- Key Encipherment

The Key Usage extension is marked as critical.

## Extensions tab — Application Policies (EKU):

- Server Authentication
# Template Comparison

## In your own words — what is the most significant difference between the User, Computer, and Web Server templates?

The User template is designed for individual user identities and supports functions such as secure email, client authentication, and Encrypting File System (EFS). The Computer template is designed for machines and supports both client and server authentication for domain-joined systems. The Web Server template is specifically designed for HTTPS and TLS server authentication. Unlike the User and Computer templates, the Web Server template allows the subject name to be supplied in the request so administrators can define DNS names manually for websites and services.

## Why does the Web Server template use "Supplied in the request" for the subject name rather than building it from Active Directory?

Web servers often host multiple websites and services that use DNS names different from the computer’s Active Directory name. Allowing the subject name to be supplied in the request lets administrators specify the exact DNS name or Subject Alternative Name (SAN) needed for HTTPS certificates. This provides flexibility for public websites, load balancers, reverse proxies, and multi-domain environments.
## Selected compatibility settings:

| Setting | Value |
|---|---|
| Certification Authority | Windows Server 2003 |
| Certificate Recipient | Windows XP / Server 2003 |
Field | Value from certutil Output
---|---
Template Name | CVI-WebServer
Template OID | (not shown in visible output — can mark as not displayed)
Schema Version | Windows Server 2003 compatibility template
Key Usage | Digital Signature, Key Encipherment
Enhanced Key Usage (EKU) | Server Authentication
Validity Period | 2 Years
Subject Name Flags | Supplied in the request
## Reflection

### Why does AD CS require you to duplicate a built-in template rather than modifying it directly?

AD CS protects built-in templates because they are default Microsoft configurations used throughout enterprise environments. Duplicating templates allows administrators to safely customize settings without breaking the original template behavior or affecting other systems relying on default configurations. This supports consistency, stability, and change control in enterprise PKI environments.

### One setting in the template you found unexpected or would want to explore further:

One setting I want to explore further is the “Supplied in the request” subject name option. It was interesting because unlike User or Computer templates that pull identity information from Active Directory automatically, the Web Server template requires the requester to manually provide the subject information. I want to better understand how SANs and DNS names are validated during real-world web server certificate enrollment.
