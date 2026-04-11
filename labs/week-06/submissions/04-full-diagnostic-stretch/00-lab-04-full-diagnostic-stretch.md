Lab 04: Full Diagnostic 

Scenario: Incident Report

Week 6 · PKI Incident Diagnosis & Troubleshooting CVI PKI Career Pathway — Phase 1 Foundations

PKI Incident Report
System: Metro General EHR Portal (ehr.metrogeneral.org)
Reported: 4/11/2026
Author: Omoze Idehenre
Status: Diagnosis complete — pending remediation

Executive Summary
Clinical staff on the new subnet (10.22.0.0/24) are unable to access the EHR system due to TLS failures, while users on the main network are unaffected. The issue is not with the certificate itself, but with trust validation on devices in the clinical subnet. The root certificate required to trust the issuing CA was not deployed to these devices, preventing them from validating the certificate chain. Remediation requires updating trust stores and validating certificate deployment across all subnets.

Technical Findings

Finding 1 — Missing Root CA in Clinical Subnet Trust Stores
Type: Trust Store
Severity: Critical

Detail:
Devices in the clinical subnet do not trust the internal Certificate Authority (Metro General Internal CA - G2) because the root certificate was not distributed to this subnet. Without the root CA, clients cannot validate the certificate chain, causing TLS failures.

Evidence:
- Clinical subnet added after Group Policy deployment
- TLS failures only occurring on 10.22.0.0/24 subnet
- Successful access from 10.10.0.0/24 confirms the certificate and server configuration are valid

---

Finding 2 — Certificate Replacement with New Key
Type: Certificate
Severity: Medium

Detail:
The certificate was fully replaced with a new key pair rather than renewed with the existing key. While not directly causing the failure, this increases operational risk if not properly tracked and validated.

Evidence:
- Scenario states full certificate replacement with a new key
- Public Key Info would differ from the previous certificate

---

Finding 3 — Lack of Post-Deployment Validation Across Subnets
Type: Configuration
Severity: High

Detail:
There was no validation step to confirm that the renewed certificate was trusted across all network segments, particularly newly added subnets.

Evidence:
- Clinical subnet added after Group Policy push
- No indication of validation testing post-deployment

---

Diagnostic Steps

Step 1 — Retrieve
Command used:

openssl s_client -connect ehr.metrogeneral.org:443 -servername ehr.metrogeneral.org

What you observed:

The certificate would be successfully retrieved. Since the system works from the main network, there is no indication of a deployment failure. The issue is not with certificate retrieval, but with validation from specific client environments.

---

Step 2 — Parse
Command used:

openssl x509 -in cert.pem -noout -subject -issuer -dates -text

Key fields checked:

- Issuer: Metro General Internal CA - G2
- Validity: 1-year window, recently issued
- Public Key Info: confirms new key pair was generated

What you found:

The certificate is valid, correctly issued, and not expired. There is no hostname mismatch or SAN issue indicated in the scenario. This rules out certificate validity as the source of failure.

---

Step 3 — Validate the Chain
Command used:

openssl verify cert.pem

Result:

Chain validation would succeed on systems with the correct root CA installed, but fail on systems missing the root certificate.

What you found:

Since the system works on the main network but fails on the clinical subnet, the certificate and chain are valid. The failure is isolated to client trust stores on the clinical subnet.

---

Step 4 — Check Revocation and Trust
Command used:

openssl x509 -in cert.pem -noout -text | grep -A3 "OCSP"

What you found:

Revocation is not the primary issue. However, because the certificate was fully replaced, the previous certificate should be evaluated for revocation. Failure to revoke could create a security risk if the old private key is compromised.

---

Failures in Diagnostic Order

1. Missing Root CA in Clinical Subnet
Type: Trust Store
Evidence: Clinical subnet added after Group Policy deployment; only this subnet experiences TLS failures

2. Lack of Trust Store Validation Across Network Segments
Type: Configuration
Evidence: No verification step after subnet expansion

3. Old Certificate Not Revoked
Type: Revocation
Evidence: Full certificate replacement performed without mention of revocation

---

Root Cause
The root cause of the incident is a gap in certificate trust distribution processes. The internal CA root certificate was deployed via Group Policy before the clinical subnet was created, resulting in devices on the new subnet lacking the required trust anchor. This was not identified because there was no validation process in place to confirm trust across newly added network segments. The issue is not a technical failure of the certificate, but an operational failure in environment synchronization and validation.

---

Remediation Steps

Immediate:
- Deploy Metro General Internal CA root certificate to all devices on the clinical subnet
- Validate TLS connectivity from affected endpoints

Short-term:
- Verify trust store consistency across all network segments
- Confirm EHR access from clinical subnet devices

Secondary:
- Revoke old certificate if no longer in use
- Update certificate inventory to reflect the new key and certificate

---

Prevention Recommendations

1. Implement automated trust store validation checks across all subnets after network changes
2. Require post-certificate deployment validation across all environments before closing change requests
3. Integrate certificate lifecycle management with network provisioning workflows to ensure trust propagation

---

Lessons Learned
This incident revealed a critical gap in how certificate trust is managed across evolving network environments. While certificate issuance and renewal processes were functioning correctly, there was no mechanism to ensure that new network segments inherited the required trust configurations. Going forward, PKI operations must be tightly integrated with infrastructure changes to prevent similar issues.

---

Reflection
The most challenging part of this investigation was recognizing that the certificate itself was not the problem, despite initial assumptions from the infrastructure team. It required discipline to follow the diagnostic framework rather than jumping to conclusions. In a real production environment, I would prioritize validating trust distribution early when issues are isolated to specific network segments.

CVI PKI Career Pathway — Foundations Phase
