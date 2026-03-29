# Week 2 Reflection — Cryptography Fundamentals

Submit this in your portfolio repository:

reflections/week-02.md

---

## Prompts

### 1. What did you learn this week?

This week helped me understand how trust is actually enforced in systems—not just conceptually, but technically.

Explain in your own words:

- The difference between confidentiality, integrity, and authenticity
- How symmetric encryption works
- How hashing detects tampering
- How digital signatures combine hashing and asymmetric cryptography

Confidentiality, Integrity, Authenticity
I learned that these are three different problems being solved:

Confidentiality is about keeping data private (only intended parties can read it)
Integrity is about making sure data hasn’t been altered
Authenticity is about proving who the data came from

Before this week, I grouped these together mentally. Now I see they are separate controls that work together.

Symmetric Encryption
Symmetric encryption uses one shared key to both encrypt and decrypt data. It’s fast and efficient, which is why it’s used for actual data transfer (like in HTTPS sessions). But the challenge is securely sharing that key in the first place.
Hashing and Tampering Detection
Hashing creates a fixed “fingerprint” of data. If even one character changes, the hash output changes completely. This is how systems detect tampering—by comparing hashes before and after transmission.

Digital Signatures
Digital signatures combine hashing and asymmetric cryptography:

Data is hashed
The hash is encrypted with a private key (the signature)
Others verify it using the public key

This ensures both integrity (data unchanged) and authenticity (verified sender). If anything is modified, the signature fails.

Avoid copying definitions. Demonstrate understanding.

---

### 2. What concept was most challenging?

Consider:

- Understanding why hashing cannot be reversed
- Grasping why digital signatures fail after tampering
- Differentiating encryption from signing

Explain what made it challenging and how you worked through it.

The most challenging concept was understanding why hashing cannot be reversed and how that impacts verification.

At first, I kept thinking of hashing like encryption, assuming there must be a way to “decode” it. But I realized hashing is intentionally one-way. It’s not meant to recover the original data—it’s meant to prove whether the data is the same.

Another challenge was understanding why digital signatures fail after tampering. In the lab, when I modified even a small part of the file, the signature verification failed. This made it clear that:

The signature is tied to the exact hash
Any change creates a completely different hash
Which breaks the verification process entirely

I worked through this by running the verification commands multiple times and intentionally modifying files to observe failure behavior.

---

### 3. Where does this appear in real-world systems?

Provide specific examples such as:

- HTTPS connections
- Code signing
- Software updates
- Certificate Authorities
- Internal enterprise PKI systems

Be concrete. Avoid vague answers.

These concepts are used everywhere in modern systems:

HTTPS Connections
When visiting a secure website, asymmetric cryptography is used to establish a secure session, then symmetric encryption is used for fast communication. Certificates and digital signatures verify the server’s identity.

Code Signing
Software (like macOS apps or updates) is digitally signed. If the code is modified or comes from an untrusted source, the system blocks execution.

Software Updates
Updates are hashed and signed to ensure they haven’t been tampered with. This prevents malicious updates from being installed.

Certificate Authorities (CAs)
CAs sign certificates to verify identity. This is how browsers trust websites—through a chain of signed certificates.

Enterprise PKI Systems
In enterprise environments, internal Certificate Authorities issue certificates for:
*Internal services
*VPN authentication
*Device identity
 This is critical for secure communication and access control.
 
---

### 4. How would you explain this topic to a non-technical audience?

Explain:

- What encryption does
- What hashing does
- What digital signatures prove

Keep it simple but accurate.

Encryption
Encryption is like putting a message in a locked box. Only someone with the key can open and read it.

Hashing
Hashing is like creating a unique fingerprint of a file. If anything changes, the fingerprint changes, so you know it’s been tampered with.

Digital Signatures
A digital signature is like signing a document with a unique, verifiable stamp. It proves who sent it and that it hasn’t been altered.

---

### 5. What questions remain?

List any uncertainties about:

- Key management
- Trust chains
- TLS
- Certificate signing
- Enterprise implementation

These questions will inform Week 3.

How are encryption keys securely stored and rotated at scale in enterprise environments?

How exactly does the trust chain work across multiple Certificate Authorities?
What happens behind the scenes during a full TLS handshake?

How do Certificate Authorities validate identity before issuing certificates?
How do large organizations manage internal PKI without introducing security risks?

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

Be prepared to inspect certificates with deeper technical understanding.
