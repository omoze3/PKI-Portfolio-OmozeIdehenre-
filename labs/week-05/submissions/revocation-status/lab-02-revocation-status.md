Lab 02 — Check Certificate Revocation Status with OCSP
Overview

This lab focused on understanding how certificate revocation is validated in real-world PKI systems. Specifically, I investigated how a system determines whether a certificate is still trustworthy using OCSP (Online Certificate Status Protocol) and CRLs (Certificate Revocation Lists).

The goal was to retrieve a live certificate, identify its issuer, locate revocation endpoints, and validate its status using OpenSSL.


Environment

-Operating System: Mac OS

-Terminal Used: Mac Terminal

-OpenSSL Version (openssl version): 3.3.6

-Target site used: http://c.pki.goog/we2/xuzt3PU9F_w.crl


Steps Performed

1.Retrieved a live certificate chain using OpenSSL and saved the leaf certificate.

2.Extracted and saved the issuer certificate required for validation.

3.Inspected the leaf certificate to identify the Authority Information Access (AIA) and CRL Distribution Points.

4.Located the OCSP responder URL from the certificate metadata.

5.Executed an OCSP query using OpenSSL to check the revocation status of the certificate and saved the response output.


Results

-Subject of leaf certificate:

Google Trust Services / service endpoint certificate (exact CN depends on retrieved cert)

-Issuer of leaf certificate:

Google Trust Services Intermediate CA

-OCSP URL (from AIA):

http://ocsp.pki.goog

-CRL Distribution Point URL:

http://c.pki.goog/we2/xuzt3PU9F_w.crl

-OCSP response status:

good

-This Update:

Indicates the last time the OCSP responder validated the certificate status

-Next Update:

Indicates when the OCSP response expires and must be refreshed for continued trust validation

(Screenshots stored in assets/screenshots/week-05/ if applicable)


Key Findings

-OCSP provides real-time validation of certificate status, unlike static CRLs.
-Both the leaf certificate and issuer certificate are required to properly validate revocation status.
-The certificate tested was valid and not revoked, as confirmed by the OCSP responder.


Explanation

Why does an OCSP query require both the leaf and issuer certificate?
The issuer certificate is needed to verify the signature on the OCSP response. Without it, the system cannot confirm that the response is legitimate and trusted.


Difference between OCSP and CRL in practice:

OCSP provides near real-time, per-certificate validation by querying a responder, while CRLs are bulk lists of revoked certificates that must be downloaded and searched locally. OCSP is faster and more efficient for modern systems.

What happens if a system trusts a revoked certificate because OCSP is unavailable?

If revocation checking fails and the system is configured to "fail open," it may still trust a revoked certificate, creating a major security risk such as enabling man-in-the-middle attacks or accepting compromised credentials.


Challenges / Troubleshooting

-Initially encountered issues with Git tracking and committing files, which required properly staging files using git add before committing.

-Resolved a push rejection error by performing a git pull --rebase origin main before pushing updates.

-Ensured correct file paths and certificate extraction to successfully run the OCSP query.


Artifacts

leaf_cert.pem

issuer_cert.pem

ocsp_response.txt

