# Lab — Symmetric Encryption (Confidentiality)

## Goal

This lab builds operational understanding of symmetric encryption and the security property of confidentiality.

You will:

- Encrypt a file using AES
- Decrypt it back to its original form
- Observe how ciphertext differs from plaintext
- Understand why symmetric encryption is used in TLS for bulk data protection

---

## Part 1 — Setup

### Prerequisites

- OpenSSL installed
- Access to a local terminal (Mac Terminal, Git Bash, or WSL)
- Your Week 2 portfolio folder created

All commands must be executed locally.  
GitHub’s web interface cannot run OpenSSL commands.

---

## Part 2 — Execution Steps

### Step 1 — Create Artifact Directory
From the root of your directory:
mkdir -p labs/02-week-02-cryptography-fundamentals/submissions/encrypted

### Step 2 — Create a Plaintext File
echo "Week 2 Symmetric Encryption Lab - CVI" > labs/02-week-02-cryptography-fundamentals/submissions/encrypted/plaintext.txt

Open the file and confirm it is readable.

### Step 3 — Encrypt the File
Use AES-256 encryption with password-based key derivation:

openssl enc -aes-256-cbc -salt -pbkdf2 \
  -in labs/02-week-02-cryptography-fundamentals/submissions/encrypted/plaintext.txt \
  -out labs/02-week-02-cryptography-fundamentals/submissions/encrypted/plaintext.txt.enc

You will be prompted for a password.

Observe:
- The encrypted file is unreadable.
- It contains binary ciphertext.

### Step 4 — Decrypt the File
openssl enc -d -aes-256-cbc -pbkdf2 \
  -in labs/02-week-02-cryptography-fundamentals/submissions/encrypted/plaintext.txt.enc \
  -out labs/02-week-02-cryptography-fundamentals/submissions/encrypted/plaintext.decrypted.txt

Enter the same password used during encryption.

### Step 5 — Verify Integrity of Decrypted File
diff labs/02-week-02-cryptography-fundamentals/submissions/encrypted/plaintext.txt \
     labs/02-week-02-cryptography-fundamentals/submissions/encrypted/plaintext.decrypted.txt

If no output appears, the files are identical.

## Part 3 — Observations
Document the following in your Week 2 lab notes:
- Why is the encrypted file unreadable?

The original plaintext is shifted utilizing AES encryption algorithm into a ciphertext. Encryption will mix the data utilizing a cryptographic key so that the contents have little to no way of being understood without the proper key or password. 

- What would happen if the wrong password were used?

The encryption key formulated from that password would not be valid. This results in the ciphertext not being correctly converted back to the original plaintext. A decryption error or unreadable output will be produced. 

- What security property does symmetric encryption provide?

Confidentiality that enforces that only those who are authorized with the correct key can read the encrypted data received. This protects the sensitive data from being accessed by unauthorized parties. 

- Why does TLS use symmetric encryption for data transfer?

Symmetric encryption is more agile than asymmetric encryption. Shortly after the TLS handshake forms the trust and securely exchanges a session key, symmetric encryption (AES, as an example) is utilized to guard the sensitive data exchanged between client and server. 


### Submission (Portfolio Repo)
Ensure the following files exist:

labs/02-week-02-cryptography-fundamentals/submissions/encrypted/
  plaintext.txt
  plaintext.txt.enc
  plaintext.decrypted.txt

Commit and push your changes.

Do NOT commit any passwords.

## Stretch (Optional)
Try decrypting the file using an incorrect password.

What error message do you receive?
Why does decryption fail?

CVI PKI Career Pathway — Foundations Phase


