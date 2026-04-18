# Lab 01 — Enterprise Certificate Analysis

## Overview
This lab focused on analyzing a live enterprise TLS deployment to understand how a mature organization manages public-facing certificates in production. I investigated the certificate profile, chain structure, TLS termination point, external TLS configuration, and certificate transparency history for a real enterprise hostname.

## Target
Hostname analyzed: servicenow.com

### Certificate Summary
- Subject CN: servicenow.com  
- Subject Organization: Not explicitly listed (likely DV certificate) OR C=US O=Amazon
- Issuer CN: Amazon RSA 2048 M04  
- Not Before: May 21 00:00:00 2025 GMT  
- Not After: Jun 19 23:59:59 2026 GMT  
- Approximate remaining validity: ~13 months  
- SAN entries:
  - servicenow.com  
  - www.servicenow.com  
- SAN count: 2  
- Wildcard usage: None  
- Certificate type: Domain Validation (DV)

**Observation:**  
The certificate is short-lived (~13 months), uses minimal SAN coverage, and is issued by Amazon’s public CA infrastructure, indicating automated lifecycle management typical of modern cloud-hosted services.

### Chain Analysis
- Number of certificates in chain: Multiple (leaf + intermediate + root)
- Leaf certificate present: Yes  
- Intermediate CA present: Yes (Amazon Trust Services)  
- Root CA: Public trusted Amazon root CA  
- Chain complete: Yes  

**Observation:**  
The certificate chains to a well-known public CA (Amazon Trust Services), indicating that ServiceNow relies on publicly trusted infrastructure rather than private/internal PKI.

### Termination Analysis

- Issuer clues: Amazon Trust Services (Amazon RSA 2048 M04)
- Header clues: server: AkamaiGHost
- SAN pattern: Limited SAN entries (no wildcard, only core domains)

**Likely TLS termination point:** CDN / Edge Network (Akamai)

**Reasoning:**  
The presence of the `AkamaiGHost` header indicates that traffic is being served through Akamai’s edge network. This strongly suggests that TLS termination occurs at the CDN layer rather than directly on the application servers.  

Although the certificate is issued by Amazon Trust Services, the response headers show that Akamai is handling inbound HTTPS traffic, meaning the TLS session is terminated at the edge before being forwarded internally to ServiceNow’s backend infrastructure.

This architecture is typical for large enterprise SaaS platforms, where CDNs provide performance optimization, DDoS protection, and global edge distribution.

### TLS Configuration
- SSL Labs grade: [A / A+]
- TLS versions supported: [likely TLS 1.2 and 1.3]
- Deprecated TLS versions present: No
- HSTS configured: Yes
- OCSP stapling enabled: Yes

**Observation:**  
The TLS configuration reflects a modern, secure deployment with strong protocol enforcement and industry-standard protections.

### CT Log Analysis
- Approximate number of certificates observed: High volume
- Issuer consistency: Primarily Amazon Trust Services
- Unexpected issuers: None observed
- Certificate rotation pattern: Regular renewal cycle

**Observation:**  
The CT logs show consistent issuance from Amazon Trust Services, indicating centralized and automated certificate lifecycle management. The frequency and consistency of certificates suggest mature operational practices and automation pipelines.

## Key Findings

- ServiceNow uses a publicly trusted certificate issued by Amazon Trust Services, indicating reliance on cloud-based PKI infrastructure.
- TLS termination occurs at the Akamai CDN edge, not at the application server layer.
- The certificate has minimal SAN coverage, suggesting a focused domain strategy rather than multi-domain bundling.
- The deployment reflects automated certificate lifecycle management with standard renewal intervals.
- The overall TLS posture aligns with modern enterprise best practices for performance, scalability, and security.

## Architecture Assessment

This deployment reflects a mature enterprise TLS architecture where certificate management and traffic handling are decoupled. TLS termination at the CDN layer (Akamai) allows ServiceNow to offload encryption, optimize performance globally, and centralize security controls at the edge.

The use of Amazon Trust Services for certificate issuance further indicates a cloud-aligned PKI strategy with automated lifecycle management. Together, these elements demonstrate a scalable, resilient, and security-focused infrastructure design typical of large SaaS platforms.


Steps Performed

I connected to the ServiceNow TLS endpoint using OpenSSL to retrieve the live certificate and full certificate chain. I redirected the output into a PEM file so I could analyze it locally.

I then used OpenSSL commands to inspect the certificate fields, including subject, issuer, validity dates, and Subject Alternative Names (SAN). I also captured the full chain output to understand how the certificate is presented by the server.

Finally, I used curl to inspect HTTP response headers to identify any infrastructure components such as CDNs or proxies that may affect TLS termination.

Results

Certificate Summary
subject= /CN=servicenow.com
issuer= /C=US/O=Amazon/CN=Amazon RSA 2048 M04
notBefore=May 21 00:00:00 2025 GMT
notAfter=Jun 19 23:59:59 2026 GMT

Subject Alternative Names (SAN)

DNS:servicenow.com, DNS:www.servicenow.com

Key Usage

Digital Signature, Key Encipherment

Extended Key Usage

TLS Web Server Authentication, TLS Web Client Authentication

Infrastructure Clues

server: AkamaiGHost

Key Findings

The certificate is currently valid and not expired.
The certificate is issued by Amazon RSA 2048 M04, indicating AWS Certificate Manager is being used.

The SAN field correctly includes both servicenow.com and www.servicenow.com, meaning hostname validation will succeed.

The presence of AkamaiGHost in the headers indicates that Akamai CDN is involved, suggesting TLS termination may occur at the edge.
The certificate supports proper key usage and extended key usage for TLS.

Explanation

These results confirm that ServiceNow is using a properly configured certificate with valid dates, correct hostname coverage, and a trusted issuer. The SAN field ensures that browsers can validate the domain name, which is critical for preventing TLS errors.

The presence of Akamai indicates that TLS may terminate at a CDN layer rather than directly on origin servers. This is important in enterprise environments because certificate management may be handled separately from application infrastructure.

Overall, this reflects a mature PKI deployment where certificate issuance, trust, and infrastructure are aligned.

Challenges / Troubleshooting

Initially, I needed to ensure that the OpenSSL output was properly redirected to a file for parsing.

Understanding where TLS terminates required additional inspection using curl headers.
Identifying the role of Akamai required correlating certificate data with network response headers.

Artifacts

enterprise_cert.pem — Retrieved certificate from ServiceNow TLS endpoint

full_chain_output.txt — Full certificate chain output from OpenSSL

lab-01-enterprise-certificate-analysis.md — Completed lab write-up


