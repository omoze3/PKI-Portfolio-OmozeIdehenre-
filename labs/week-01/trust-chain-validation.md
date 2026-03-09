# Week 01 Mini Lab — Trust Chain Validation

## Screenshot Evidence

Capture a screenshot of the Certification Path (certificate chain) from your browser.

Save it as:

assets/screenshots/week-01/trust-chain-validation.png

Embed the screenshot below:

![Trust Chain Validation](../../assets/screenshots/week-01/trust-chain-validation.png)


## Website Information

**Website inspected:**  
<!-- Enter full URL -->
https://www.hotplate.com/confirmation?cartId=5e895e80-fe89-4073-b696-51b0fb8ca89c&id=b191d082-ae8e-42b4-b1e3-95ae9b4f75d3&utm_source=message24HoursBefore_sms
---

## Certificate Chain Breakdown

**Leaf (Server) Certificate**  
<!-- Enter [assets_screenshots_week-01_trust-chain-validation (Updated).png.pdf](https://github.com/user-attachments/files/25794349/assets_screenshots_week-01_trust-chain-validation.Updated.png.pdf)
Common Name or Subject -->
www.hotplate.com

**Intermediate Certificate Authority**
<!-- Enter Intermediate CA name -->
R12
**Root Certificate Authority (Trust Anchor)**
<!-- Enter Root CA name -->
ISRG Root X1
---

## Trust Anchor Verification
[assets_screenshots_week-01_trust-chain-validation (Updated).png.pdf](https://github.com/user-attachments/files/25794341/assets_screenshots_week-01_trust-chain-validation.Updated.png.pdf)

Is the Root CA marked as trusted by your system?

<!-- Yes / No -->
Yes

If yes, explain where that trust comes from (OS/browser root store).

Root Store 

If no, explain what warning or behavior occurred.

---

## Observations

Document three observations about the certificate.

### Observation 1
<!-- What did you notice about the chain structure? -->
The Leaf Certificate appears first, and then the Intermediate. Some work must be done to identify the root certificate. 
### Observation 2
<!-- What did you notice about the Root CA? -->
It doesn't appear in the certificate located in the browser, or it may not be as obvious to find for someone who would not understand how to look. 
### Observation 3
<!-- What did you notice about how the browser determines trust? -->
Trust was validated throughout the chain. No security warning was displayed. 
---

## Reflection

In 3–5 sentences, explain:
- Why the Root certificate is called a trust anchor 
- How validation walks the certificate chain
- What would happen if the Root CA were not trusted
- 
The Root certificate is stored in the OS or a browser's root certificate store, and it is trusted by default. Any certificates that can be traced back to it through a valid chain are also trusted. A browser will connect to a secure website and first check the server certificate. The browser will then verify that an intermediate certificate authority issued the server certificate. It then verifies that the intermediate certificate was issued by a trusted Root CA. If each of the certificates are valid and correctly signed, the browser accepts the connection and establishes the secure HTTPS communication. If the system does not trust the Root CA, then the browser will not be able to validate the certificate chain. A security warning would then be displayed by the browser to indicate that the connection is not secure or that the certificate can't be trusted. Browsers will often block the connection unless the user manually overrides the warning. 
Use your own words.
