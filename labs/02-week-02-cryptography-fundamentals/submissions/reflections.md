# Week 2 Reflection — Cryptography Fundamentals

Submit this in your portfolio repository:

reflections/week-02.md

---

## Prompts

### 1. What did you learn this week?

Explain in your own words:

- The difference between confidentiality, integrity, and authenticity
- 
- Confidentiality: Protects the secrecy of sensitive information so unauthorized people cannot read it. During the symmetric encryption lab, the plaintext file became an unreadable ciphertext after AES encryption, showing how encryption protects data from being read by other parties.

- Integrity: Ensures that data has not been altered. In the hashing lab, generating a SHA-256 hash created a unique fingerprint of the file. After modification of the file, adding a single word, the new hash was completely different. This demonstrated how hashing can detect tampering.

- Authenticity: Proves who created or approved data. In the digital signature lab, a private key was used to sign a file, and the public key verified that signature. Because only the private key holder could create the signature, verification confirms the identity of the signer.
- 
- How symmetric encryption works

- It uses a singular shared key to encrypt and decrypt data. The same key used to scramble the plaintext into ciphertext is also used to convert it back into readable text.
- 
- How hashing detects tampering

- A small change to the input produces a completely different hash value. This allows systems to quickly check if data has been modified.
- 
- How digital signatures combine hashing and asymmetric cryptography

- It first hashes the data and then encrypts the hash with a private key. When someone verifies the signature with the public key, they then confirm both the integrity of the data and the identity of the signer. 

Avoid copying definitions. Demonstrate understanding.

---

### 2. What concept was most challenging?

Consider:

- Understanding why hashing cannot be reversed
- Grasping why digital signatures fail after tampering
- Differentiating encryption from signing

Explain what made it challenging and how you worked through it.

The most challenging concept was understanding why hashing cannot be reversed. At first, it seemed similar to encryption, where data is transformed but can later be decrypted. Hashing works differently because it produces a one-way fingerprint of the data rather than a reversible one. Since many different inputs could produce the same length output, the original data cannot be reconstructed from the hash. 
---

### 3. Where does this appear in real-world systems?

Provide specific examples such as:

- HTTPS connections
- Code signing
- Software updates
- Certificate Authorities
- Internal enterprise PKI systems

Be concrete. Avoid vague answers.

Code signing uses digital signatures to prove that software came from a trusted developer and has not been modified. 

In HTTPS connections, symmetric encryption protects the data being transferred between the browser and the server, while certificates and signatures verify the identity of the website. 

Software updates use hashing and signatures to verify that update packages have not been tampered with during download. 

---

### 4. How would you explain this topic to a non-technical audience?

Explain:

- What encryption does

- It's like locking evidence of a deep, painful family secret into a buried lock box under the basement so only someone with the specific details of the location can open it with a specific key designed for that box in that specific location.
- 
- What hashing does

- It's like creating a unique and detailed photo of someone with certain shapes and colors tailored to that person's face and personality. If the color or shapes change, even a little, the entire photo changes.
- 
- What digital signatures prove

- It's like signing a document w/ a unique pen that only you own. Anyone can check your signature to confirm that the document really came from you and has not been changed.

- When you put all of this together, these tools allow systems to protect information, detect changes, and verify who created or approved the data. 

Keep it simple but accurate.

---

### 5. What questions remain?

List any uncertainties about:

- Key management
- Trust chains
- TLS
- Certificate signing
- Enterprise implementation

These questions will inform Week 3.

How do organizations securely manage and store private keys at scale? 

Can I receive more clarity on how TLS uses both asymmetric and symmetric cryptography together during the handshake and session encryption? 

---

## Professional Growth Check

- Did you explain concepts in your own words?
- Did you connect theory to practice?
- Did you reference your lab observations?
- Is your formatting clean and structured?
- Was your commit message meaningful?

---

## Forward Link

Week 2 introduced the mechanisms that enforce digital trust.

Week 3 will show how X.509 certificates combine encryption, hashing, and digital signatures into a structured trust model.

Be prepared to inspect certificates with a deeper technical understanding.
