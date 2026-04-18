# Lab 01 — Convert Certificate Formats

## Overview
This lab focused on working with the same certificate in different storage formats. I retrieved a live website certificate, converted it from PEM to DER, converted it back to PEM, and created a PFX bundle using a test certificate and private key. The lab showed how certificates can be represented differently without changing the underlying trust information.

---

## Environment
- Operating System: macOS
- Terminal Used: Terminal
- OpenSSL Version: 3.3.6

---

## Steps Performed
1. Created the Week 4 convert-formats submission folder.

2. Retrieved Google’s leaf certificate and saved it as `leaf_cert.pem`.

   openssl s_client -connect google.com:443 -servername google.com \
   | openssl x509 -outform PEM > leaf_cert.pem

3. Verified the PEM certificate with `openssl x509 -text -noout`.

   openssl x509 -in leaf_cert.pem -text -noout

4. Converted the PEM certificate into DER format and validated it.

   openssl x509 -in leaf_cert.pem -outform DER -out leaf_cert.der

5. Converted the DER certificate back into PEM and compared the original and restored files.

   openssl x509 -in leaf_cert.der -inform DER -out leaf_cert_restored.pem

6. Compared the original and restored certificates:

   diff leaf_cert.pem leaf_cert_restored.pem
   
7. Generated a test RSA private key and self-signed certificate.

   openssl genrsa -out test_key.pem 2048
   openssl req -new -x509 -key test_key.pem -out test_cert.pem -days 365

8. Bundled the test certificate and private key into a PFX file.

   openssl pkcs12 -export -out test_bundle.pfx -inkey test_key.pem -in test_cert.pem

9. Verified the PFX contents with `openssl pkcs12 -info`.

   openssl pkcs12 -info -in test_bundle.pfx

---

## Results
- The PEM file was readable text with `BEGIN CERTIFICATE` and `END CERTIFICATE` markers.
  -----BEGIN CERTIFICATE-----
  
- The DER file appeared as binary and was not human-readable in a text editor.
- The restored PEM matched the original after converting PEM → DER → PEM.
- The PFX verification confirmed that the bundle was created successfully and contained certificate material protected by a password.
- openssl s_client -connect google.com:443 -servername google.com \
| openssl x509 -text -noout



---

## Key Findings
- PEM is text-based and easy to inspect
- DER is binary and compact
- PFX bundles certificate + private key for transport
- The diff command produced no output, confirming:
     * PEM → DER → PEM conversion was lossless
     * No data or trust information was altered

---

## Explanation
A PFX requires a password because it contains sensitive key material. PEM is useful when working with servers and command-line tools because it is readable and easy to copy. DER is useful when a binary encoded certificate is required. PFX is useful when moving a certificate and its private key to another system, especially in enterprise or Windows-based environments. Private keys should never be committed to GitHub because anyone with the key could impersonate the identity associated with the certificate.

PEM is ideal for servers and manual inspection
DER is used when binary encoding is required
PFX is used to transport certificates with private keys
Security Insight
PFX requires a password because it contains sensitive key material
Private keys must never be committed to GitHub
Anyone with a private key can impersonate that identity

---

## Challenges / Troubleshooting
The main challenge was retrieving only the leaf certificate cleanly from the TLS connection output. I resolved this by piping the `openssl s_client` output directly into `openssl x509`, which extracted just the certificate and avoided copy/paste errors.

openssl s_client ... | openssl x509

This:

Extracted only the certificate
Avoided formatting errors
Improved accuracy and efficiency

---

## Artifacts
- convert-formats/certs/leaf_cert.pem
- convert-formats/certs/leaf_cert.der
- convert-formats/certs/leaf_cert_restored.pem
- convert-formats/certs/test_cert.pem
- convert-formats/artifacts/test_bundle.pfx
