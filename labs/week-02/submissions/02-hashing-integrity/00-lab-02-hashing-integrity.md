# Lab — Hashing & Integrity

## Overview
This lab focused on understanding how cryptographic hashing works and how it is used to ensure data integrity. The goal was to observe how even a small change in a file produces a completely different hash and to understand how hashing is used in PKI systems.

---

## Environment

- Operating System: macOS
- Terminal Used: Mac Terminal
- OpenSSL Version: OpenSSL 3.x

---

## Steps Performed

1. Created a test file containing sample text data.

   echo "This is a test message" > message.txt
   
2. Generated a SHA-256 hash of the file using OpenSSL and saved the output.

   openssl dgst -sha256 message.txt > message.sha256.txt
   
3. Modified the original file by adding additional content.

   echo "This is a test message with a change" > message.txt
   
4. Generated a second SHA-256 hash of the modified file.

   openssl dgst -sha256 message.txt > message_tampered.sha256.txt
   
5. Compared the original and modified hash outputs to observe differences.

---

## Results

- The original file produced a fixed-length SHA-256 hash.
- After modifying the file, the new hash value was completely different.
- Even a small change in the file resulted in a drastically different hash output.
- This confirmed that hashing is highly sensitive to input changes.

Example observation:
- Original hash ≠ Tampered hash

<img width="1168" height="404" alt="Lab 2 Week 2" src="https://github.com/user-attachments/assets/e89908ff-2de1-49c9-b171-2b99c2e1ec2a" />

---

## Key Findings

- Cryptographic hashes produce a fixed-length output regardless of input size.
- Even the smallest change in data results in a completely different hash value.
- Hashing is designed to detect tampering or changes in data.
- Hashing does not encrypt or hide the original data.

---

## Explanation

These results matter because hashing is a core mechanism for verifying data integrity in PKI systems. When data is hashed, any modification can be detected by comparing hash values. This is critical in digital signatures, certificate validation, and secure communications. However, hashing does not provide confidentiality, since the original data is still visible and not encrypted.

Key Insight

Hashing allows systems to verify:

“Has this data changed?”

But it does NOT answer:

“Can someone read this data?”

That’s encryption’s role.

---

## Challenges / Troubleshooting

- Ensured correct OpenSSL syntax when generating hash values.
- Verified file paths to ensure the correct files were hashed.
- Confirmed that file modification occurred before generating the second hash.

---

## Artifacts

- message.txt (original file)
- message.sha256.txt (original hash)
- message_tampered.sha256.txt (modified hash)
- lab-02-hashing-integrity.md (this write-up)

---

