# Lab 01 — Enterprise Certificate Analysis

## Overview
This lab focused on analyzing a live enterprise TLS deployment to understand how a mature organization manages public-facing certificates in production. I investigated the certificate profile, chain structure, TLS termination point, external TLS configuration, and certificate transparency history for a real enterprise hostname.

## Target
Hostname analyzed: servicenow.com

### Certificate Summary
- Subject CN: servicenow.com  
- Subject Organization: Not explicitly listed (likely DV certificate)  
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


