# Lab 05 — Extract a Certificate from a Live Website

## Overview
This lab focused on retrieving a live TLS certificate from a website and inspecting its fields and extensions using OpenSSL. The PKI concept being investigated was how certificates identify websites, define trust, and support TLS validation.

---

## Environment
- OS: macOS
- Terminal used: Mac Terminal
- OpenSSL version: 
- Website used: github.com

---

## Certificate Fields Found

| Field                    | Value from your output |
|--------------------------|------------------------|
| Subject                  | CN=github.com |
| Issuer                   | Sectigo ECC Domain Validation Secure Server CA |
| Not Before               | Mar 10 00:00:00 2026 GMT |
| Not After                | Jun 10 23:59:59 2026 GMT |
| Public Key Algorithm     | id-ecPublicKey |
| Subject Alternative Name | github.com, www.github.com |
| Key Usage                | Digital Signature |
| Extended Key Usage       | TLS Web Server Authentication, TLS Web Client Authentication |

<img width="618" height="453" alt="Extract a Certificate from a Live Website 1" src="https://github.com/user-attachments/assets/0f9c03a1-c3de-4118-bfcb-4002f38be33f" />

<img width="621" height="442" alt="Extract a Certificate from a Live Website 2" src="https://github.com/user-attachments/assets/f290ff75-37c4-4622-a438-717094e89f2f" />


---

## Observations

1. What organization does this certificate belong to?
   This certificate belongs to GitHub and is issued to github.com.

2. Which Certificate Authority issued it?
   The certificate was issued by Sectigo ECC Domain Validation Secure Server CA.

3. When does it expire? Jun 10 23:59:59 2026 GMT
   The expiration date is listed in the Not After field of the certificate.

4. What domains are listed in the SAN field?
   The SAN field includes github.com and www.github.com.

5. What is this certificate authorized to be used for?
   This certificate is authorized for TLS Web Server Authentication and TLS Web Client Authentication.
