# Lab 02 — Inspect Your Trust Store

## Overview
This lab explored how macOS stores trusted root Certificate Authorities (CAs) and how those certificates are used to validate secure HTTPS connections. The goal was to understand how trust is pre-established through the system trust store and how certificate validation works in practice.

---

## Environment
- Operating System: macOS
- Terminal Used: Mac Terminal
- OpenSSL Version: 3.3.6

---

## Steps Performed
1. Created a directory to store the trust store inspection artifacts
2. Exported the trusted root certificates from the macOS System Root Keychain into a PEM file
3. Counted the number of trusted root certificates in the exported bundle
4. Used OpenSSL to inspect a root certificate’s subject, issuer, and expiration date
5. Validated a live HTTPS certificate from Google against the system trust store

---

## Results
- Number of root CAs: 154

- Example Root CA:
  - Subject: /CN=DigiCert Global Root CA/OU=www.digicert.com/O=DigiCert Inc/C=US
  - Issuer: /CN=DigiCert Global Root CA/OU=www.digicert.com/O=DigiCert Inc/C=US
  - Expiration: Nov 10 00:00:00 2031 GMT

- Example Root CA:
  - Subject: /C=US/ST=Arizona/L=Scottsdale/O=GoDaddy.com, Inc./CN=Go Daddy Root Certificate Authority - G2
  - Issuer: /C=US/ST=Arizona/L=Scottsdale/O=GoDaddy.com, Inc./CN=Go Daddy Root Certificate Authority - G2
  - Expiration: Dec 31 23:59:59 2037 GMT
 
- Verify Output:
  - Verify return code: 0 (ok)


This confirms that the system trusts Google’s certificate because it chains up to a trusted root Certificate Authority already installed on the system.


![Uploading C«USST«ArizonaL«Scottsdale0«GoDaddy.com, Inc.CN«Go Daddy Root Cert.png…]()

![Trust Store Inspection](../../../assets/screenshots/week-04/lab-02-trust-store.png)

The output confirms that the system trust store contains valid root CA certificates used to verify trusted connections.

---

## Key Findings
- macOS comes preloaded with many trusted root Certificate Authorities
- Root certificates are self-signed, so the subject and issuer are often the same
- Trust is established through a chain from the server certificate to an intermediate CA and then to a trusted root CA
- HTTPS validation depends on the contents of the local trust store

---

## Explanation
Browsers trust certificates from websites you have never visited before because those websites present certificates that chain back to a root CA already trusted by the operating system.

If an enterprise's internal root CA were not installed in the trust store, internal sites, proxies, or services signed by that CA would fail validation and show certificate warnings or connection errors.

One important observation from this lab is how many root CAs are already trusted by default. That demonstrates how much trust is delegated to external certificate authorities.

---

## Challenges / Troubleshooting
Initially, the certificate fields were not easy to read when using truncated OpenSSL output. I resolved this by using explicit flags to print the subject, issuer, and end date directly.

I was also a little bit confused about which root certificates to apply to the write up. I saw 2 for GoDaddy and DigiCert

---

## Artifacts

The following artifact was generated during this lab to demonstrate inspection of the system trust store:

- artifacts/root_cas.pem  

Extracted bundle of trusted root Certificate Authorities from the system trust store. This file was used to analyze how operating systems maintain and validate trusted certificate chains.

### Artifact Context

This artifact represents the foundation of trust in PKI systems. By inspecting the system trust store, we can observe how trusted root Certificate Authorities are preloaded and used to validate certificate chains during secure communications (e.g., HTTPS).

Understanding this trust store is critical for:
- Verifying certificate authenticity
- Troubleshooting trust failures
- Managing enterprise trust policies



