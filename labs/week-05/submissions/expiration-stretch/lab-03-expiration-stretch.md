Lab 03 — Simulate a Certificate Expiration Scenario (Stretch)

Overview

This lab focused on simulating certificate expiration to understand how expired certificates behave in real-world environments. The goal was to observe how expiration is reflected in certificate metadata, how systems validate certificate validity, and how to replace an expired certificate.

This lab reinforced that certificate expiration is predictable but still a common cause of outages when not properly monitored.

Environment

Operating System: MacOS

Terminal Used: Mac Terminal

OpenSSL Version: 3.3.6


Steps Performed

1.Generated a private key and Certificate Signing Request (CSR) for a test certificate.

2.Created a short-lived certificate to observe a valid but limited validity window.

3.Attempted to generate an expired certificate using standard OpenSSL commands and encountered compatibility issues with the Mac (LibreSSL/OpenSSL behavior differences).

4.Created a temporary Certificate Authority (CA) configuration to manually issue a certificate with a past validity window.

5.Successfully generated an expired certificate by explicitly setting the start and end dates in the past.

6.Verified certificate validity using OpenSSL commands.

7.Generated a replacement certificate to simulate certificate rotation.

8.Removed temporary private key material and ensured no sensitive files were committed to Git.


Results

Expired Certificate Created:

test_cert_expired.pem

Short-Lived Certificate Created:

test_cert_short.pem

Replacement Certificate Created:

test_cert_replacement.pem

Certificate Validity Check:

openssl x509 -in test_cert_expired.pem -dates -noout

Output showed:

notBefore: Date in the past

notAfter: Date in the past

Expiration Validation Check:

openssl x509 -in test_cert_expired.pem -checkend 0

Result confirmed the certificate is expired.

Key Findings

Certificate expiration is entirely predictable based on the validity window.

Tools and environments (OpenSSL vs LibreSSL) can behave differently and impact implementation.

Expired certificates still exist structurally but fail validation checks.

Replacement certificates must be generated and deployed before expiration to avoid service disruption.


Explanation

Why expiration matters:

Systems rely on certificate validity windows to establish trust. Once a certificate expires, it is no longer considered trustworthy, even if the cryptographic structure is intact.

Why replacement is required instead of renewal:

Certificates are not “extended” — they must be reissued and redeployed.


Real-world impact:

If a certificate expires in production, systems relying on it (websites, APIs, internal services) can fail, causing outages, security warnings, or broken connections.

Challenges / Troubleshooting

The -days 0 flag failed on the Mac environment due to LibreSSL/OpenSSL differences.

Direct certificate generation using openssl x509 did not support setting custom validity windows in this context.

Resolved by using openssl ca with a custom configuration file to explicitly define start and end dates.

Ensured private keys were not committed to version control by removing temporary files and validating tracked files.


Artifacts

test_cert_short.pem

test_cert_expired.pem

test_cert_replacement.pem

test_csr.pem

replacement_csr.pem

Final Outcome

This lab provided hands-on experience with certificate lifecycle management, specifically expiration and replacement. It highlighted how small operational gaps (like missed expiration) can lead to major system failures and reinforced the importance of monitoring, automation, and proactive certificate management.













