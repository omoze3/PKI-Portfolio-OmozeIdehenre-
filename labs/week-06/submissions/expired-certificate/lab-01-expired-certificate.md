Lab 01 — Diagnose an Expired Certificate
Week 6 · PKI Incident Diagnosis & Troubleshooting CVI PKI Career Pathway — Phase 1 Foundations

Incident Summary
Target system: portal.metrogeneral.org (simulated via expired.badssl.com)
Diagnosed by: Omoze Idehenre
Date of diagnosis: [2026-04-10]

What failed
The TLS failure was caused by an expired leaf certificate served by the target system.

Evidence
- Not Before: [paste value]
- Not After: [paste value]
- Subject: [paste subject]
- Issuer: [paste issuer]
- Certificate expired [X] days ago at the time of testing.
- OpenSSL verify/connection output showed an expiration-related error.
- Chain status: [structurally intact / note actual finding]

Why it failed
The certificate was outside its validity period because the current date was later than the Not After value. During TLS validation, clients reject certificates that are no longer valid, so the connection fails even if the certificate otherwise matches the host and the chain is present. This is a certificate lifecycle failure consistent with the expiration failure type covered in Week 5 and diagnosed using the Week 6 PKI Diagnostic Framework.

Chain status
[Write what you actually found. Example: The certificate chain appeared structurally complete, with the server presenting the leaf and intermediate certificate(s). I did not identify a separate chain construction problem beyond the expired certificate.]

Remediation path
1. Confirm the expired leaf certificate currently deployed on the portal.
2. Generate a new CSR if required by the CA or internal issuance workflow.
3. Request or issue a replacement certificate for the portal hostname.
4. Install the new certificate on the web server.
5. Confirm any required intermediate CA certificate is included in the server configuration.
6. Reload or restart the service so the new certificate is presented.
7. Re-test with openssl s_client and confirm the new certificate is within its validity period.
8. Validate browser access and remove the patient-facing security warning.

Prevention
Implement certificate inventory and automated expiration alerts so the team is notified well before the Not After date.

Diagnostic Steps

Step 1 — Retrieve
Command used:

openssl s_client -connect expired.badssl.com:443 -showcerts </dev/null 2>/dev/null \
  | openssl x509 -outform PEM > expired_cert.pem

What you observed:
The command [did/did not] successfully retrieve a PEM-encoded certificate. The connection output indicated [paste error / verify return code].

Step 2 — Parse
Command used:

openssl x509 -in expired_cert.pem -text -noout
openssl x509 -in expired_cert.pem -noout -dates
openssl x509 -in expired_cert.pem -noout -subject -issuer

Key fields from the certificate:

| Field | Value |
|---|---|
| Subject CN | [paste value] |
| Issuer | [paste value] |
| Not Before | [paste value] |
| Not After | [paste value] |
| SAN entries | [paste SANs if present] |

What you found:
The certificate was expired because the current date was later than the Not After timestamp. This directly explains the TLS failure.

Step 3 — Validate the Chain
Command used:

openssl s_client -connect expired.badssl.com:443 -showcerts </dev/null 2>/dev/null

Result:
[Example: Chain structurally present; expiration remained the primary validation failure.]

What you found:
This step confirmed whether there were any chain-related issues separate from the certificate expiration. [State your actual result.]

Step 4 — Check Revocation and Trust
Command used:

openssl x509 -in expired_cert.pem -noout -text | grep -A1 "OCSP"

What you found:
[OCSP URL present / absent]. Revocation was not the primary remediation concern because an expired certificate is already invalid and must be replaced regardless of revocation status.

Reflection
This lab reinforced the importance of checking certificate validity dates early instead of guessing based on generic browser warnings. The parsing step was the key moment because the Not After field gave the exact reason for the TLS failure. In a real incident, I would still walk the full framework to rule out chain or trust issues before concluding the diagnosis.






