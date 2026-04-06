Lab — Digital Signatures (Integrity + Authenticity)

Why verification succeeds before tampering

Verification succeeds because the data has not been changed, so the hash of the original message matches the hash that was signed with the private key. When the public key is used to verify the signature, both values align, confirming the data is authentic and unchanged.

Why verification fails after modification

Verification fails because even a small change in the data produces a completely different hash. The new hash no longer matches the original signed hash, so the signature cannot be validated, indicating the data has been altered.

Why do digital signatures require both hashing and asymmetric cryptography?

Hashing is used to create a fixed, efficient representation of the data, while asymmetric cryptography is used to securely sign and verify that hash. Without hashing, signing large data would be inefficient, and without asymmetric cryptography, there would be no secure way to verify identity. Together, they ensure both integrity and authenticity.

How does this relate to certificate signing in PKI?

In PKI, Certificate Authorities (CAs) use digital signatures to sign certificates. They hash the certificate data and sign it with their private key. When a browser or system receives the certificate, it uses the CA’s public key to verify the signature, ensuring the certificate is valid and has not been tampered with.
