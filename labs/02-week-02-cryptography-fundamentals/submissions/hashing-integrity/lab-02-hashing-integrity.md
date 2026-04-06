Lab — Hashing & Integrity (Observations)

Why did the hash change after a small modification

A cryptographic hash function is designed so that even the smallest change in the input produces a completely different output. This is called the avalanche effect, where a tiny modification causes a drastic change in the hash value.
Why hashing does NOT provide confidentiality

Why hashing does NOT provide confidentiality

Hashing does not hide or encrypt data. The original data is still readable, and hashing only creates a fixed-length fingerprint of that data. Anyone can see the original content, so it does not protect against unauthorized viewing.

What security property does hashing provide?

Hashing provides data integrity. It allows systems to detect whether data has been altered because any change in the input results in a different hash value.

Where hashing is used in PKI systems

Digital signatures: A hash of the message is signed with a private key
Certificate signing: Certificate Authorities hash certificate data before signing it
TLS handshakes: Hashing verifies the integrity of handshake messages
Certificate fingerprints: Hashes uniquely identify certificates
