# Lab — Symmetric Encryption

## Overview
This lab focused on understanding how symmetric encryption works and how it is used to protect data confidentiality. The goal was to explore how a single shared key can be used to both encrypt and decrypt data, and how this concept is applied in real-world systems like TLS.

---

## Environment

- Operating System: macOS
- Terminal Used: Mac Terminal
- OpenSSL Version: OpenSSL 3.x

---

## Steps Performed

1. Created a plaintext file to simulate sensitive data.
2. Used OpenSSL to encrypt the file using a symmetric encryption algorithm.
3. Generated an encrypted output file and confirmed the contents were unreadable.
4. Used the same key to decrypt the file and verify the original content was restored.

---

## Results

- The original plaintext file was successfully encrypted into unreadable ciphertext.
- The encrypted file could only be decrypted using the same key that was used to encrypt it.
- When decrypted correctly, the file returned to its original readable form.
- This demonstrated that symmetric encryption protects data confidentiality during storage or transmission.


Example observation:
- Plaintext → readable
- Encrypted file → unreadable
- Decrypted file → matches original

<img width="587" height="76" alt="Week 2 Lab 1 " src="https://github.com/user-attachments/assets/fe719d6d-c882-4119-98ed-3e3faab337ee" />

---

## Key Findings

- Symmetric encryption uses a single shared key for both encryption and decryption.
- It is highly efficient and fast, making it suitable for encrypting large amounts of data.
- The biggest risk is secure key distribution, since both parties must have the same key.
- If the key is exposed, the encrypted data can be compromised.

---

## Explanation

These results matter because symmetric encryption is widely used in real-world systems like HTTPS (TLS) to protect data in transit. While asymmetric encryption is used to establish secure connections, symmetric encryption is used afterward for performance reasons. This highlights the importance of key management, as the security of the system depends entirely on keeping the shared key secret.

---

## Challenges / Troubleshooting

- Initially needed to ensure the correct OpenSSL command syntax for encryption and decryption.
- Verified that the same key and parameters were used during decryption to avoid errors.
- Confirmed file paths and output files to ensure encryption and decryption worked properly.

---

## Artifacts

- Encrypted file generated using OpenSSL
- Original plaintext file
- Decrypted output file
- lab-01-symmetric-encryption.md (this write-up)

---

*CVI PKI Career Pathway — Foundations Phase*
