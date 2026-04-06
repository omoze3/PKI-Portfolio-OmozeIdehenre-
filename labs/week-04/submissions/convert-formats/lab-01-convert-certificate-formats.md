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
3. Verified the PEM certificate with `openssl x509 -text -noout`.
4. Converted the PEM certificate into DER format and validated it.
5. Converted the DER certificate back into PEM and compared the original and restored files.
6. Generated a test RSA private key and self-signed certificate.
7. Bundled the test certificate and private key into a PFX file.
8. Verified the PFX contents with `openssl pkcs12 -info`.

---

## Results
- The PEM file was readable text with `BEGIN CERTIFICATE` and `END CERTIFICATE` markers.
- The DER file appeared as binary and was not human-readable in a text editor.
- The restored PEM matched the original after converting PEM → DER → PEM.
- The PFX verification confirmed that the bundle was created successfully and contained certificate material protected by a password.
- openssl s_client -connect google.com:443 -servername google.com \
| openssl x509 -text -noout
<img width="558" height="319" alt="Serial Number" src="https://github.com/user-attachments/assets/af071131-e770-4980-a3d1-d89d7ace0c1d" />


---

## Key Findings
- PEM is text-based and easy to inspect manually.
- DER is binary and often used when a compact encoded format is needed.
- PFX packages a certificate together with its private key and is commonly used for transport and import into systems.
- The diff command produced no output, confirming the conversion round-trip was lossless.

---

## Explanation
A PFX requires a password because it contains sensitive key material. PEM is useful when working with servers and command-line tools because it is readable and easy to copy. DER is useful when a binary encoded certificate is required. PFX is useful when moving a certificate and private key together into another system, especially in enterprise or Windows-based environments. Private keys should never be committed to GitHub because anyone with the key could impersonate the identity associated with the certificate.

---

## Challenges / Troubleshooting
The main challenge was retrieving only the leaf certificate cleanly from the TLS connection output. I resolved this by piping the `openssl s_client` output directly into `openssl x509`, which extracted just the certificate and avoided copy/paste errors.

---

## Artifacts
- convert-formats/certs/leaf_cert.pem
- convert-formats/certs/leaf_cert.der
- convert-formats/certs/leaf_cert_restored.pem
- convert-formats/certs/test_cert.pem
- convert-formats/artifacts/test_bundle.pfx
