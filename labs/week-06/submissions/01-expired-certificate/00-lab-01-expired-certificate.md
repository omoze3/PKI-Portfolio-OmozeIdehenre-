Lab 01 — Diagnose an Expired Certificate
Week 6 · PKI Incident Diagnosis & Troubleshooting CVI PKI Career Pathway — Phase 1 Foundations

Incident Summary
Target system: portal.metrogeneral.org (simulated via expired.badssl.com)
Diagnosed by: Omoze Idehenre
Date of diagnosis: 2026-04-10

What failed
The TLS failure was caused by an expired leaf certificate served by the target system.

Evidence
- Not Before: Apr  9 00:00:00 2015 GMT
- Not After: Apr 12 23:59:59 2015 GMT
- Subject: OU=Domain Control Validated, OU=PositiveSSL Wildcard, CN=*.badssl.com
- Issuer: C=GB, ST=Greater Manchester, L=Salford, O=COMODO CA Limited, CN=COMODO RSA Domain Validation Secure Server CA
- Certificate expired 4,000+ days ago (11 years ago) at the time of testing.
- OpenSSL connection output showed a certificate expiration error, indicating the certificate is no longer valid.
- Chain status: Structurally intact (no missing intermediate certificates observed; failure is due to expiration only)

Why it failed
The certificate was outside its validity period because the current date was later than the Not After value. During TLS validation, clients reject certificates that are no longer valid, so the connection fails even if the certificate otherwise matches the host and the chain is present. This is a certificate lifecycle failure consistent with the expiration failure type covered in Week 5 and diagnosed using the Week 6 PKI Diagnostic Framework.

Chain status
The certificate chain appeared structurally complete, with the server presenting the leaf certificate and intermediate certificate(s). There were no missing components in the chain, and no separate chain validation errors were observed. The TLS failure was solely due to the certificate being expired.

Remediation path
1. Identify the expired certificate currently deployed on the portal.
2. Generate a new Certificate Signing Request (CSR) for the correct hostname if required.
3. Request or issue a replacement certificate from the certificate authority (CA).
4. Install the new certificate on the server hosting the portal.
5. Ensure the correct intermediate certificate(s) are included in the server configuration.
6. Restart or reload the web service to apply the new certificate.
7. Validate the fix using openssl s_client to confirm a successful TLS handshake.
8. Verify in a browser that the security warning is resolved and the connection is trusted.

Prevention
Implement certificate lifecycle management (CLM) with automated expiration monitoring and alerting at defined intervals. Configure alerts to trigger at 90, 60, and 30 days before certificate expiration. 

At 90 days, notify the responsible owner to review the certificate and confirm renewal plans. At 60 days, escalate the alert to ensure renewal is in progress. At 30 days, trigger high-priority alerts and initiate automated renewal where possible.

Additionally, assign clear ownership for all certificates and maintain a centralized inventory to ensure no certificate is left unmanaged.

If expiration notifications are missed, secondary controls must detect and prevent the failure.

Implement automated certificate discovery scans that run daily to identify certificates nearing expiration or already expired, regardless of alert delivery.

Configure monitoring systems to perform live TLS checks on critical services and trigger alerts if a certificate expires or a handshake fails.

Additionally, enforce ownership escalation policies so that if no action is taken at the 30-day mark, the issue is automatically escalated to management or on-call engineers.

Where possible, implement automatic certificate renewal to eliminate reliance on manual intervention.

Diagnostic Steps

Step 1 — Retrieve
Command used:

openssl s_client -connect expired.badssl.com:443 -showcerts </dev/null 2>/dev/null \
  | openssl x509 -outform PEM > expired_cert.pem

What you observed:

The command successfully retrieved a PEM-encoded certificate and saved it as expired_cert.pem. 

The OpenSSL connection output showed a certificate validation failure, with a verify return code indicating that the certificate had expired (certificate has expired).

Step 2 — Parse
Command used:

openssl x509 -in expired_cert.pem -text -noout
openssl x509 -in expired_cert.pem -noout -dates
openssl x509 -in expired_cert.pem -noout -subject -issuer

Key fields from the certificate:

| Field | Value |
|---|---|
| Subject CN | *.badssl.com |
| Issuer |  C=GB, ST=Greater Manchester, L=Salford, O=COMODO CA Limited, CN=COMODO RSA Domain Validation Secure Server CA|
| Not Before | Apr  9 00:00:00 2015 GMT |
| Not After | Apr 12 23:59:59 2015 GMT |
| SAN entries | DNS:*.badssl.com, DNS:badssl.com |

What you found:

Certificate expired 4,000+ days ago (~11 years)
OpenSSL returned: certificate has expired
Chain status: Structurally intact
Certificate identity is correct for the domain
Certificate is expired → root cause of TLS failure

This confirms the failure is due to certificate expiration, not chain or trust issues.
The certificate had expired because the current date was later than the Not After timestamp. This directly explains the TLS failure.

Step 3 — Validate the Chain
Command used:

openssl s_client -connect expired.badssl.com:443 -showcerts </dev/null 2>/dev/null

Result:
The certificate chain was structurally present. The server returned the leaf certificate along with the intermediate certificate(s), and no missing chain elements were observed. The primary validation failure remained the expired certificate.

What you found:
This step confirmed that no chain-related issues were contributing to the failure. The chain could be built correctly, and the root CA was trusted. The TLS failure was isolated to the certificate being expired, not a chain construction or trust issue.

Chain presented correctly
No missing intermediates

What This Proves
Chain is structurally valid
Failure is not chain-related

Step 4 — Check Revocation and Trust
Command used:

openssl x509 -in expired_cert.pem -noout -text | grep -A1 "OCSP"

What you found:
An OCSP URL was present in the certificate, indicating that revocation status could be checked if needed. However, revocation was not relevant to the remediation in this case because the certificate had already expired. An expired certificate is automatically considered invalid by TLS clients, so it must be replaced regardless of its revocation status.

Key Insight

Revocation is not relevant here because:

Expiration is evaluated first
Expired certificates are automatically invalid

Even a “good” revocation status would not fix this

Reflection
This lab reinforced the importance of checking certificate validity dates early instead of guessing based on generic browser warnings. The parsing step was the key moment because the Not After field gave the exact reason for the TLS failure. In a real incident, I would still walk the full framework to rule out chain or trust issues before concluding the diagnosis.

Mental Model

Identity + Trust + Verification

Identity = Subject + SAN
Trust = Certificate chain
Verification = Validity period + trust

Artifacts

expired_cert.pem




