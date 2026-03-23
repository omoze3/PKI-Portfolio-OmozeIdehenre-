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

* Verification of the digital signature failed when the file content was modified.
* This demonstrated that even a small change invalidates the signature.

Example observation:
- Valid signature → verification succeeds  
- Modified data → verification fails  

<img width="578" height="147" alt="Lab 3 Week 2" src="https://github.com/user-attachments/assets/b56bee96-e2b6-4652-ba53-dd796433d0cf" />

---

## Key Findings

* Modified data → verification fails
Verification of the digital signature failed when the file content was modified.
This demonstrated that even a small change invalidates the signature.

---

## Explanation

Digital signatures include a hash of the original file. When the file changes, the hash changes, so the signature no longer matches. This proves: Integrity protection and Tamper detection

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

