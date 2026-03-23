# Lab — Digital Signatures

## Overview
This lab focused on understanding how digital signatures work by combining hashing and asymmetric cryptography to ensure data integrity and authenticity. The goal was to observe how a message can be signed with a private key and verified using a public key.

---

## Environment

- Operating System: macOS
- Terminal Used: Mac Terminal
- OpenSSL Version: OpenSSL 3.x

---

## Steps Performed

1. Created a file containing sample data to act as the message.
2. Generated a hash of the file using OpenSSL.
3. Created a digital signature by signing the hash with a private key.
4. Used the corresponding public key to verify the digital signature.
5. Confirmed that any modification to the file would cause signature verification to fail.

---

## Results

- A digital signature was successfully created using the private key.
- The signature was verified using the public key, confirming authenticity.
- When the original file remained unchanged, verification succeeded.
- If the file were modified, the verification would fail.

Example observation:
- Valid signature → verification succeeds  
- Modified data → verification fails  

[PKI Week 2 Artifacts .pdf](https://github.com/user-attachments/files/26174786/PKI.Week.2.Artifacts.pdf)
---

## Key Findings

- Digital signatures use hashing to create a fixed representation of data.
- The hash is encrypted with a private key to create the signature.
- The public key is used to verify the signature and confirm authenticity.
- Digital signatures ensure both integrity and identity.

---

## Explanation

These results matter because digital signatures are a critical component of PKI systems. They ensure that data has not been altered and confirm the identity of the sender. This process is used in certificate signing, secure communications, and software distribution. Without digital signatures, systems would not be able to verify trust or detect tampering.

---

## Challenges / Troubleshooting

- Ensured correct use of private and public keys during signing and verification.
- Verified command syntax for creating and validating signatures.
- Confirmed that the correct file was used during verification to avoid errors.

---

## Artifacts

- Original message file
- Signature file generated using private key
- Public key used for verification
- lab-03-digital-signatures.md (this write-up)

---

*CVI PKI Career Pathway — Foundations Phase*

