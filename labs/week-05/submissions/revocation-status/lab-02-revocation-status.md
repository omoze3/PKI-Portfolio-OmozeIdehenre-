# Lab 02 — Revocation Status Check

## OCSP Responder URL

http://ocsp.pki.goog/we2

---

## CRL Distribution Point

http://c.pki.goog/we2/xuzt3PU9F_w.crl

---

## OCSP Command Used

```bash
openssl ocsp \
  -issuer issuer_cert.pem \
  -cert leaf_cert.pem \
  -url "$(openssl x509 -in leaf_cert.pem -noout -ocsp_uri)" \
  -noverify \
  -text \
  > ocsp_response.txt

OCSP Response Output (Key Fields)
OCSP Response Status: successful (0x0)
Produced At: Apr 3 10:54:59 2026 GMT
Cert Status: good
This Update: Apr 3 10:54:59 2026 GMT
Next Update: Apr 10 09:54:58 2026 GMT

OCSP Responder URL
http://ocsp.pki.goog/we2

CRL Distribution Point
http://c.pki.goog/we2/xuzt3PU9F_w.crl

Certificate Status
good

Produced At
Apr 3 10:54:59 2026 GMT

This Update
Apr 3 10:54:59 2026 GMT

Next Update
Apr 10 09:54:58 2026 GMT

OCSP Response Status
successful (0x0)

Command Used
openssl ocsp \
  -issuer issuer_cert.pem \
  -cert leaf_cert.pem \
  -url "$(openssl x509 -in leaf_cert.pem -noout -ocsp_uri)" \
  -noverify \
  -text \
  > ocsp_response.txt
Interpretation
The OCSP response confirms that the certificate is currently valid and has not been revoked. The "Cert Status: good" result indicates that the issuing Certificate Authority has not marked this certificate as compromised or invalid.
The "Produced At" and "This Update" timestamps show when the OCSP response was generated, while the "Next Update" value indicates when the client should check again for updated revocation information.

CRL vs OCSP
Certificate Revocation Lists (CRLs) and Online Certificate Status Protocol (OCSP) are both mechanisms used to check whether a certificate has been revoked.
CRLs require downloading an entire list of revoked certificates, which can be large and inefficient. OCSP, on the other hand, allows real-time validation of a single certificate by querying the Certificate Authority directly.
OCSP is generally preferred in modern systems because it is faster, more efficient, and provides near real-time revocation status.


Challenges / Troubleshooting
During the lab, the initial OCSP query returned a response verification failure because the OCSP responder’s signing certificate was not available locally.
To proceed, the query was rerun using the -noverify flag, which allowed inspection of the certificate’s revocation status without validating the responder’s certificate chain.
This highlights that OCSP verification may require additional trusted certificates in a real-world environment, but the certificate status itself can still be retrieved successfully.

Summary
This lab demonstrated how to extract certificates from a live TLS connection, identify OCSP and CRL endpoints, and verify certificate revocation status using OpenSSL.
The certificate tested was confirmed to be valid and not revoked.



