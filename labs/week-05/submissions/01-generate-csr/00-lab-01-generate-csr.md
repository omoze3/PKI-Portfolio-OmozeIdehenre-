Lab 1
# Lab 01 — Generate a CSR

## Overview
In this lab, I generated a private key and corresponding Certificate Signing Request (CSR), then used that CSR to create a signed certificate. This exercise demonstrates the foundational Public Key Infrastructure (PKI) workflow used in secure communications.
The lab reinforces how identity is established and verified through cryptographic trust chains.


---

## Environment
- Operating System: MacOS
- Terminal Used: OpenSSL
- OpenSSL Version: 3.3.6

---

## Steps Performed
1. Created the Week 5 Lab 01 submission folder.

openssl genrsa -out test_key.pem 2048

2. Generated a 2048-bit RSA private key for the test certificate request. This includes identity details such as:

Created CSR containing:
* Common Name (CN)
* Organization (O)
* Country (C)

CSR links:
* Identity (subject details)
* Public key

openssl req -new -key test_key.pem -out test_csr.pem

3. To simulate a Certificate Authority (CA), I signed the CSR using the private key to generate a certificate.

openssl x509 -req -in test_csr.pem -signkey test_key.pem -out test_cert.pem -days 365

The following files were generated:
* test_key.pem → Private key (sensitive, removed from repo)
* test_csr.pem → Certificate Signing Request
* test_cert.pem → Signed certificate

The private key was intentionally removed from the repository to follow security best practices.

Simulated a Certificate Authority (CA)
Signed the CSR to produce a certificate valid for 365 days


4. I verified that the certificate was successfully generated and readable:
openssl x509 -in test_cert.pem -text -noout

This confirmed:
* The certificate contains the expected subject information
* The certificate is properly formatted
* The validity period is correctly applied

5. Validate Key Pair

Extract Public Key from Private Key

openssl rsa -in test_key.pem -pubout -out key_pub.pem

Extract Public Key from Certificate

openssl x509 -in test_cert.pem -pubkey -noout > cert_pub.pem

Compare Keys

diff key_pub.pem cert_pub.pem

Result

No output -> Keys match

What This Proves

The certificate corresponds to the correct private key
The key pair is valid and usable for TLS

If a CSR is generated using one private key, but the server is configured with a different private key:

The certificate will still be issued successfully
BUT the server cannot complete TLS handshakes
Production Failure Scenario
Browser error: TLS handshake failure
The server cannot decrypt traffic
Outage occurs despite a “valid” certificate

This validation step prevents deploying a non-functional certificate


6. Security Considerations
* Private keys must never be committed to source control
* .gitignore was configured to exclude .key files
* The private key (test_key.pem) was removed after use
* Certificates and CSRs can be shared, but private keys must remain secure

7. Real-World Application
This workflow mirrors how secure systems operate in production:
* A server generates a private key and CSR
* The CSR is sent to a trusted Certificate Authority (CA)
* The CA validates identity and signs the certificate
* The signed certificate is used to enable HTTPS and encrypted communication
This process is foundational in:

* Web security (TLS/SSL)
* Enterprise identity systems
* Cloud infrastructure security

7. Key Takeaways
* A CSR links identity to a public key
* The private key must remain protected at all times
* Certificates establish trust through signatures
* PKI enables secure communication across untrusted networks

Mental Model
Private Key = Identity ownership CSR = Request for trust Certificate = Verified identity

Artifacts

Generated Files:

test_csr.pem

Certificate Signing Request containing:
Subject identity details (CN, O, C)
Public key associated with the private key
Used to request certificate issuance

test_cert.pem

Self-signed certificate generated from the CSR:

Valid for 365 days
Contains verified identity + public key
Represents a completed issuance workflow


