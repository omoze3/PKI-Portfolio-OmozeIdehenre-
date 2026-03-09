# Week 01 Lab — Key Pair Generation

## Screenshot Evidence

If using OpenSSL:
1. Capture a screenshot showing:
  - The command used to generate the private key
  - The command used to extract the public key
2. Save it as:

**assets/screenshots/week-01/keypair-generation.png**

3. Embed the screenshot below:
<img width="582" height="365" alt="assets:screenshots:week-01:keypair-generation png  " src="https://github.com/user-attachments/assets/d3392964-ab01-451f-b043-ce5d9b9feb73" />

**![Key Pair Generation](../../assets/screenshots/week-01/keypair-generation.png)**

If using a browser-based generator, capture the generated key pair screen (redact sensitive portions of the private key before committing).

---

## Key Identification
**Which file is the public key?**
<!-- Example: public.key -->
public_key.pem
**Which file is the private key?**
<!-- Example: private.key -->
private_key.pem
---

## Key Properties
Briefly describe:
- What makes the public key safe to share
- What makes the private key sensitive
The public key is not private, but public and created to be shareable. It can not decrypt data or produce signatures without aligning with the private key. It can not perform these actions on its own. 
---
The private key is not shared and allows the owner to decrypt encrypted data from the public key. It not only decrypts data from the public key, but it also formulates signatures. If someone retrieves access to the private key, they can impersonate the owner to obtain personal data, such as information from encrypted messages/details meant just for the owner of the key. 
## Security Scenario
What would happen if someone obtained your private key?

Explain the risk in terms of:
  - Identity
  - Impersonation
  - Trust
  - 
If someone were to obtain my private key, they could obtain access to my private information and act as me to obtain further personal information. That private key acts as my digital identity. PKI is so effective because websites/systems can trust that the owner holds the private key. If someone (not the owner) were to take hold of that private key, then trust is automatically broken. The key must be revoked and replaced to restore trust. 
---

## Observations
Document three observations from this lab.

### Observation 1
<!-- What did you notice about key generation? -->
The importance of redacting the private key. Truly understanding a real-world scenario to understand the importance of a private key. 
### Observation 2
<!-- What did you notice about key size or format? -->
The private key seemed smaller/shorter than the longer public key. 
### Observation 3
<!-- What did you notice about how the keys differ? -->
The public key encrypts data or verifies signatures. It can not decrypt or generate signatures without the private key. The private key decrypts the encrypted data and generates signatures. 
---

## Reflection
In 3–5 sentences, explain:

Why must the private key remain secret in a PKI system?
The private key is our personal digital identity. Without full ownership, someone can use it to impersonate the owner and retrieve personal information designed to be private. They could create websites utilizing one's identity to illegally obtain information and even funds. 
Focus on how identity is tied to possession of the private key.
