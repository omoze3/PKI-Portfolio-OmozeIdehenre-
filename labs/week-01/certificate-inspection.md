# Week 01 Lab — Certificate Inspection

## Screenshot Evidence

1. Capture a screenshot of the certificate details in your browser.
2. Save it as:

assets/screenshots/week-01/certificate-inspection.png

3. Embed the screenshot below:

![Certificate Inspection](../../assets/screenshots/week-01/certificate-inspection.png)
<img width="538" height="674" alt="assets:screenshots:week-01:certificate-inspection png " src="https://github.com/user-attachments/assets/0cbcc975-21e5-4211-8ed2-095ea8794a21" />


## Website Information

**Website inspected:**  
<!-- Enter full URL -->
https://www.youtube.com/watch?v=Vxkncr53bJU
**Issuer (Certificate Authority):**  
<!-- Example: DigiCert, Let's Encrypt, GlobalSign -->
Google Trust Services (WE2)
**Valid from:**  
<!-- Start date -->
Monday, February 2, 2026 at 12:36:42 AM
**Valid until:**  
<!-- Expiration date -->
Monday, April 27, 2026 at 1:36:41 AM
**Signature algorithm:**  
<!-- Example: sha256WithRSAEncryption -->

--- X9.62 ECDSA Signature with SHA-256

## Subject Alternative Names (SAN Entries)

List at least 2–3 SAN entries:

- DNS Name: *.google.com
- DNS Name: *.appengine.google.com
- DNS Name: *.bdn.dev

---

## Observations

Document three observations about the certificate.

### Observation 1
<!-- What did you notice? --> 
The length of the validity period is almost 3 months 
### Observation 2
<!-- What did you notice? -->
The pairing of the SHA-256 fingerprints as the Certificate and Public Key
### Observation 3
<!-- What did you notice? -->
The lengthy list of DNS Names/SANS and the issuer aligns with the set of SANS/DNS Names
---

## Reflection

Based on your inspection, explain how this certificate contributes to secure HTTPS communication.
Due to the details on the certificate, one can deduce that the website it belongs to is owned by the domain *.google.com. Therefore, legitimate. The Issuer Google Trust Services is also listed, along with the encrypted public key. This will likely ensure the secure exchange of the encryption keys (Public and Private). 
(2–3 sentences)
