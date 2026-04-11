Week 6 Lesson Notes — PKI Incident Diagnosis & Troubleshooting

1. Core Concept  
PKI incident diagnosis is a structured approach to identifying why a TLS connection fails. Instead of guessing, engineers follow a repeatable framework: retrieve the certificate, parse its contents, validate the chain of trust, and check revocation and trust configuration. The goal is to isolate whether the failure is due to the certificate itself, the chain, the hostname identity, or the client’s trust environment.

---

2. Why It Matters  
In real enterprise environments, TLS failures directly impact availability, security, and user trust. A single certificate issue can take down patient portals, payment systems, or internal tools. Engineers must quickly determine whether the issue is expiration, misconfiguration, missing intermediates, or trust store problems. Without a structured approach, teams waste time guessing instead of resolving incidents efficiently.

---

3. Technical Breakdown  

Definition  
PKI troubleshooting is the process of validating certificate authenticity, trust chains, and identity matching to determine why a secure connection fails.

Components  
- Leaf Certificate (server identity)  
- Intermediate Certificate(s) (chain builders)  
- Root Certificate Authority (trust anchor)  
- Trust Store (client-side trusted roots)  
- SAN (Subject Alternative Name for hostname validation)  
- Revocation (OCSP / CRL)

Flow  
1. Client connects to server  
2. Server presents certificate chain  
3. Client verifies:
   - Certificate validity (Not Before / Not After)  
   - Chain completeness (leaf → intermediate → root)  
   - Trust (root exists in trust store)  
   - Identity (hostname matches SAN)  
4. If any step fails → TLS connection fails

Trust Implications  
Trust is not just about having a certificate — it is about proving identity through a chain that ends in a trusted authority. If any link is missing or untrusted, the entire connection is rejected. Trust is enforced by the client, not the server.

---

4. Common Misconceptions  

Misconception 1  
“If the certificate is valid, TLS will work.”  
Reality: A certificate can be valid but still fail due to missing intermediates or hostname mismatch.

Misconception 2  
“If OpenSSL says ‘verify return code: 0’, everything is fine.”  
Reality: OpenSSL does not enforce hostname validation by default. Identity mismatch can still exist.

Misconception 3  
“TLS failures are always server issues.”  
Reality: Many failures are client-side, such as missing root certificates in trust stores.

---

5. Where This Shows Up  

Web Security  
- Browser warnings (expired cert, untrusted issuer, hostname mismatch)  
- HTTPS failures are blocking user access  

Internal Enterprise Systems  
- EHR systems, APIs, and internal dashboards are failing due to trust store issues  
- Group Policy or MDM misconfigurations affecting certificate trust  

Cloud Environments  
- Load balancers are missing full certificate chains  
- Misconfigured TLS termination in AWS, Azure, or GCP  

DevOps Workflows  
- CI/CD pipelines failing due to certificate validation errors  
- Improper certificate deployment or missing intermediates in containers  

---

Mental Model  

TLS = Identity + Trust + Verification  

- Identity → Does the certificate match the system I’m connecting to?  
- Trust → Can I trace this certificate back to a trusted authority?  
- Verification → Do all validation steps (time, chain, hostname) pass?  

If any one of these fails, the connection fails.
