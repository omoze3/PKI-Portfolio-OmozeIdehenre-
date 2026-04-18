Lab 03 — Diagnose a Hostname and SAN Mismatch
Week 6 · PKI Incident Diagnosis & Troubleshooting CVI PKI Career Pathway — Phase 1 Foundations

Incident Summary

Target system: Staff scheduling portal (simulated via wrong.host.badssl.com)

Diagnosed by: Omoze Idehenre

Date of diagnosis: 4/11/2026

What failed: 

TLS validation failed due to a hostname mismatch between the requested domain and the certificate’s Subject Alternative Name (SAN) entries.

Evidence
- Subject CN: *.badssl.com
- SAN entries: *.badssl.com, badssl.com
- Requested hostname: wrong.host.badssl.com
- OpenSSL verify return code: 0 (ok), indicating the certificate chain is valid
- The requested hostname does not match any SAN entries in the certificate

Why it failed:
The certificate presented by the server was valid, not expired, and chained to a trusted certificate authority. However, the hostname being accessed (wrong.host.badssl.com) was not included in the certificate’s Subject Alternative Name (SAN) entries. Because TLS requires an exact hostname match against the SAN field, the client rejected the connection despite the certificate being otherwise valid. This reflects the identity validation step in the PKI diagnostic framework.

Chain status
The certificate chain was structurally intact and validated successfully. There were no issues related to certificate expiration or missing intermediates. The TLS failure was caused solely by a hostname mismatch.

Chain validity: Valid
Certificate expiration: Valid
Intermediate CA: Present
Hostname match: Failed

Remediation path
1. Identify the hostname being accessed by users.
2. Generate a Certificate Signing Request (CSR) that includes the correct hostname in the SAN field.
3. Submit the CSR to a trusted certificate authority.
4. Issue a new certificate that includes all required hostnames.
5. Install the updated certificate on the server.
6. Restart or reload the service to apply the new certificate.
7. Validate the fix using OpenSSL and browser testing to confirm the hostname matches the certificate.

Why a DNS CNAME Will NOT Fix This (Required Section)

A DNS CNAME only changes where a request is routed—it does not change how the browser validates identity.

For example:

A user types: staff.metrogeneral.org
DNS may point it to another server
BUT the browser still checks the certificate against staff.metrogeneral.org

If that hostname is not listed in the certificate’s SAN:

TLS validation fails

Key Point (Non-Technical Explanation)

Think of DNS like forwarding mail to a new address—but the ID being checked still has your original name on it.

The certificate is the browser’s source of truth for identity.
The only real fix is issuing a certificate that includes the correct hostname.

Prevention

Ensure all required hostnames are included in the SAN field during certificate issuance. Implement validation checks during deployment to confirm hostname coverage before promoting certificates to production.

Diagnostic Steps

Step 1 — Retrieve
Command used:

openssl s_client -connect wrong.host.badssl.com:443 -servername wrong.host.badssl.com -showcerts </dev/null 2>/dev/null > san_full.txt
awk 'BEGIN {c=0} /BEGIN CERTIFICATE/ {c++} c==1 {print}' san_full.txt > san_cert.pem

What you observed:

The certificate was successfully retrieved. The -servername flag ensured the correct certificate was returned instead of a fallback certificate.

Step 2 — Parse
Command used:

openssl x509 -in san_cert.pem -noout -subject -issuer -dates
openssl x509 -in san_cert.pem -noout -text | grep -A5 "Subject Alternative Name"

Key fields from the certificate:

| Field | Value |
|------|------|
| Subject CN | *.badssl.com |
| Issuer | Let's Encrypt |
| Not Before | Mar 24 20:02:52 2026 GMT |
| Not After | Jun 22 20:02:51 2026 GMT |
| SAN entries | *.badssl.com, badssl.com |

What you found:

The certificate was valid and covered *.badssl.com and badssl.com. However, it did not include wrong.host.badssl.com in the SAN field.

Step 3 — Validate the Chain
Command used:

openssl s_client -connect wrong.host.badssl.com:443 -servername wrong.host.badssl.com </dev/null 2>&1 | grep "Verify return code"

Result:

Verify return code: 0 (ok)

What you found:

This confirmed that the certificate chain was valid and trusted. There were no issues with certificate expiration or missing intermediates.

Step 4 — Check Revocation and Trust
Command used:

Extract OCSP URL

openssl x509 -in san_cert.pem -noout -ocsp_uri

Result

(no output)

What you found:

No OCSP URL is provided in the certificate
Revocation status cannot be checked via OCSP

The SAN entries included *.badssl.com and badssl.com, but did not include wrong.host.badssl.com. This confirmed the mismatch between the requested hostname and the certificate identities. Revocation was not relevant because the certificate itself was valid.

Conclusion
Certificate is structurally valid
No revocation information available via OCSP
Failure is strictly due to hostname mismatch

Reflection

This lab reinforced that TLS validation has two separate checks:

Trust validation (chain of trust)
Identity validation (hostname match)

Even when trust is valid, identity failure alone is enough to break TLS.

The key learning was understanding that:

A certificate can be valid but incorrect
SAN entries define identity—not the CN alone
DNS cannot override certificate identity
Mental Model

Identity + Trust + Verification

Identity = SAN entries
Trust = Certificate chain
Verification = Matching hostname + valid chain

Artifacts

san_cert.pem
san_full.txt
