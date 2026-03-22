# Lab — Hashing & Integrity

## Goal

This lab builds operational understanding of cryptographic hashing and the security property of integrity.

You will:

- Generate a SHA-256 hash of a file
- Modify the file
- Generate a new hash
- Observe how even small changes completely alter the hash value

---

## Part 1 — Setup

### Prerequisites

- OpenSSL installed
- Access to a local terminal
- Week 2 portfolio folder created

All commands must be executed locally.

---

## Part 2 — Execution Steps

### Step 1 — Create Artifact Directory

From the root of your repository:

mkdir -p labs/02-week-02-cryptography-fundamentals/submissions/hashes

### Step 2 — Create a Test File
echo "Week 2 Hashing Lab - CVI" > labs/02-week-02-cryptography-fundamentals/submissions/hashes/message.txt

Open the file and confirm it is readable.

### Step 3 — Generate a SHA-256 Hash
openssl dgst -sha256 labs/02-week-02-cryptography-fundamentals/submissions/hashes/message.txt \
> labs/02-week-02-cryptography-fundamentals/submissions/hashes/message.sha256.txt

Open the hash file and observe:
- A fixed-length output
- A hexadecimal string
- The algorithm used (SHA-256)

### Step 4 — Modify (Tamper With) the File
echo "tampered" >> labs/02-week-02-cryptography-fundamentals/submissions/hashes/message.txt

Even a single character change is enough.

### Step 5 — Generate a New Hash
openssl dgst -sha256 labs/02-week-02-cryptography-fundamentals/submissions/hashes/message.txt \
> labs/02-week-02-cryptography-fundamentals/submissions/hashes/message_tampered.sha256.txt

Compare the two hash outputs.

They should be completely different.

## Part 3 — Observations
Document the following in your Week 2 notes:
- Why the hash changed after a small modification

- The function of a cryptographic hash is designed to detect the smallest change in the input data produced. The slightest modification changes the hash.  

- Why hashing does NOT provide confidentiality

- It doesn't hide or encrypt the original data. The sensitive data remains readable. Hashing doesn't protect data from being viewed. It just creates a fixed-length fingerprint of the data that can be utilized for verification.

- What security property does hashing provide?

- Integrity of data. Hashing allows systems to detect any alteration(s) in data. If a file's hash changes, it indicates a file modification.

- Where hashing is used in PKI systems

- Digital Signatures: Hash of message signed w/ a private key.

- Certificate Signing: CAs hash certificate data before signing.

- TLS Handshakes: Hashing helps verify the integrity of handshake messages. 

- Certificate Fingerprints: Hashes are used to identify certificates uniquely. 

Examples to consider:
- Certificate signatures
- File integrity validation
- Code signing

### Submission (Portfolio Repo)
Ensure the following files exist:

labs/02-week-02-cryptography-fundamentals/submissions/hashes/
  message.txt
  message.sha256.txt
  message_tampered.sha256.txt

Commit and push your changes.

Do not upload screenshots unless explicitly requested.

## Stretch (Optional)
Try using a different hashing algorithm:

openssl dgst -sha512 message.txt

- How does the output length compare?

- SHA-512 formulates a 512-bit hash (128 hex characters), which is longer than SHA-256 (256 bits / 64 hex characters) and SHA-1 (160 bits / 40 hex characters). 

- Why are weak hashing algorithms (like SHA-1) no longer recommended?

- SHA-1 is vulnerable to collision attacks, where different inputs can produce the same hash. This breaks data integrity and makes it insecure for modern systems like PKI and TLS.
  
CVI PKI Career Pathway — Foundations Phase

CVI PKI Career Pathway — Foundations Phase

