Lab 02 — Diagnose a Broken Certificate Chain
Week 6 · PKI Incident Diagnosis & Troubleshooting CVI PKI Career Pathway — Phase 1 Foundations

Incident Summary
Target system: Radiology imaging platform (simulated via incomplete-chain.badssl.com)

Diagnosed by: Omoze Idehenre

Date of diagnosis: 4/10/2026

What failed
TLS validation failed because the server did not provide the intermediate certificate needed to complete the certificate chain.

Evidence
- Subject CN: *.badssl.com
- Issuer CN: Let's Encrypt R13
- Not Before: Mar 24 20:02:52 2026 GMT
- Not After: Jun 22 20:02:51 2026 GMT
- OpenSSL verification failed with: error 20 at 0 depth lookup: unable to get local issuer certificate
- Authority Information Access showed: CA Issuers - URI:http://r13.i.lencr.org/
- After adding the missing intermediate certificate, OpenSSL verification succeeded

Why it failed
The leaf certificate itself was valid and within its validity period, so the failure was not caused by expiration or hostname mismatch. The problem was that the server did not send the intermediate certificate required to link the leaf certificate to a trusted root CA. This caused TLS validation to fail because the client could not build a complete chain of trust, which is exactly the type of chain failure covered in the Week 6 PKI diagnostic framework.

Chain status
The chain was broken. The leaf certificate was present and valid, but the required intermediate certificate, Let's Encrypt R13, was not being provided by the server. There were no separate expiration issues or hostname mismatch issues in this scenario; the TLS failure was caused by the incomplete chain alone.

Remediation path
1. Retrieve the leaf certificate from the affected server.
2. Inspect the Issuer field and the Authority Information Access extension to identify the missing intermediate certificate.
3. Download the missing intermediate certificate from the CA Issuers URI.
4. Convert the intermediate certificate to PEM format if it is delivered in DER format.
5. Install the intermediate certificate on the server alongside the leaf certificate so the server presents the full chain.
6. Reload or restart the web service so the updated certificate chain is served.
7. Re-test the system using openssl s_client and openssl verify to confirm the chain validates successfully.
8. Confirm that the client can now complete the chain to a trusted root CA without errors.

Prevention
Implement a post-renewal certificate deployment checklist that includes full-chain validation before a renewed certificate is placed into production. In addition, maintain certificate inventory and automate validation checks to ensure required intermediate certificates are installed after every renewal.

Diagnostic Steps
Document each step of the PKI Diagnostic Framework as you worked through it.

Step 1 — Retrieve
Command used:

openssl s_client -connect incomplete-chain.badssl.com:443 -servername incomplete-chain.badssl.com -showcerts </dev/null 2>/dev/null > fullchain.txt
awk 'BEGIN {c=0} /BEGIN CERTIFICATE/ {c++} c==1 {print}' fullchain.txt > leaf_cert.pem

What you observed:

The certificate was successfully retrieved and saved to leaf_cert.pem. Using -servername was necessary to avoid pulling the fallback certificate and to retrieve the correct certificate for incomplete-chain.badssl.com.

Step 2 — Parse
Command used:

openssl x509 -in leaf_cert.pem -noout -subject -issuer -dates
openssl x509 -in leaf_cert.pem -noout -text | grep -A3 "Authority Information Access"

Key fields from the certificate:

| Field | Value |
|---|---|
| Subject CN | *.badssl.com |
| Issuer | /C=US/O=Let's Encrypt/CN=R13 |
| Not Before | Mar 24 20:02:52 2026 GMT |
| Not After | Jun 22 20:02:51 2026 GMT |
| SAN entries | *.badssl.com, badssl.com |

What you found:

The leaf certificate was valid and not expired. The Issuer field identified Let's Encrypt R13 as the intermediate CA. The Authority Information Access extension provided a CA Issuers URI, confirming where the missing intermediate certificate could be retrieved.

Step 3 — Validate the Chain
Command used:

openssl verify leaf_cert.pem

Result:

Chain broken. OpenSSL returned:
error 20 at 0 depth lookup: unable to get local issuer certificate
leaf_cert.pem: verification failed: 20 (unable to get local issuer certificate)

What you found:

This confirmed that the client could not build a complete chain of trust from the leaf certificate to a trusted root. The failure was not in the leaf certificate itself, but in the absence of the required intermediate certificate.

Step 4 — Check Revocation and Trust
Command used:

openssl x509 -in leaf_cert.pem -noout -text | grep -A3 "Authority Information Access"
curl -s http://r13.i.lencr.org/ -o intermediate.der
openssl x509 -inform DER -in intermediate.der -out issuer_cert.pem
openssl verify -untrusted issuer_cert.pem leaf_cert.pem

What you found:

The Authority Information Access extension included a CA Issuers URI pointing to the missing intermediate certificate. After downloading and converting the intermediate certificate, verification succeeded when it was supplied with the -untrusted flag. This confirmed that the problem was an incomplete chain, not a revocation issue or an untrusted root problem.

Reflection
This lab reinforced the importance of validating the full certificate chain instead of assuming that a valid leaf certificate means the TLS configuration is correct. I had to slow down and think carefully about the difference between a certificate problem and a server configuration problem. The key lesson was that a certificate can be perfectly valid and still fail in production if the server does not present the required intermediate CA.

CVI PKI Career Pathway — Foundations Phase
