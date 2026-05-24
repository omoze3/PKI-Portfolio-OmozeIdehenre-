# Lab 03: HSM Key Ceremony — Observation Report

**Student Name:** Omoze Idehenre  
**Date Completed:** 2026-05-23  
**Phase:** 2 | **Week:** 10  
**Submission Path:** labs/week-10/lab-03-hsm-observation.md

---

# Overview

A Hardware Security Module (HSM) is a specialized device used to securely generate, store, and protect cryptographic keys. Enterprise Certificate Authorities use HSMs to protect highly sensitive CA private keys from theft, export, or unauthorized access. In this demonstration, SoftHSM2 was used as a software-based HSM simulator to demonstrate PKCS#11 key ceremony concepts without requiring expensive enterprise hardware.

---

# Part A — Setup Context

## What is SoftHSM2?

SoftHSM2 is a software-based HSM simulator that mimics the behavior of a physical Hardware Security Module. It allows administrators and students to practice secure key management and PKCS#11 operations in a lab environment. It was used in this demonstration because physical enterprise HSMs such as Thales Luna devices are expensive and require specialized hardware and licensing.

## What is PKCS#11?

PKCS#11 is a cryptographic interface standard that allows software applications to communicate with HSMs and cryptographic tokens. It provides a common method for performing cryptographic operations such as key generation, encryption, signing, and key storage regardless of the hardware vendor.

## HSM Being Simulated in This Demo

| Item | Value |
|---|---|
| Software used | SoftHSM2 |
| PKCS#11 library path | `/opt/homebrew/lib/softhsm/libsofthsm2.so` |
| Companion tool used (key ceremony commands) | `pkcs11-tool` |

---

# Part B — Step-by-Step Observation

## Step 1 — Token Slot Initialization

### Command run by instructor:

```bash
softhsm2-util --init-token --slot 0 \
--label "CVI-CAKeyStore" \
--pin 1234 \
--so-pin 5678
```

### What this command does:

This command initializes a new SoftHSM2 token in slot 0. It creates a secure token container, assigns a label, and configures both the user PIN and Security Officer PIN.

### Output or confirmation observed:

```bash
The token has been initialized and is reassigned to slot 2076269524
```

### Why this step is necessary:

A token must be initialized before cryptographic keys can be securely stored. This process creates the protected storage area that simulates an HSM partition in an enterprise environment.

---

## Step 2 — Confirm Slot/Token State

### Command run:

```bash
softhsm2-util --show-slots
```

### Output:

```bash
Available slots:
Slot 2076269524

Token info:
Label: CVI-CAKeyStore
Initialized: yes
User PIN init.: yes
```

### What did the output confirm?

The output confirmed that the token was successfully initialized, assigned a label, and configured with a user PIN. It also showed that the token was active and ready for cryptographic operations.

---

## Step 3 — Key Generation

### Command run:

```bash
pkcs11-tool --module /opt/homebrew/lib/softhsm/libsofthsm2.so \
--login --pin 1234 \
--keypairgen --key-type rsa:2048 \
--label "CVI-IssCA-Key" \
--id 01
```

## Key parameters used

| Parameter | Value |
|---|---|
| Key type | RSA |
| Key size | 2048 bits |
| Key label | CVI-IssCA-Key |
| Token | CVI-CAKeyStore |

### Output:

```bash
Key pair generated:
Private Key Object: RSA 2048 bits
Public Key Object: RSA 2048 bits
Label: CVI-IssCA-Key
ID: 01
```

### What this step accomplished:

This step generated a new RSA 2048-bit key pair directly inside the SoftHSM2 token. The private key remained stored inside the token and was marked as non-extractable.

---

## Step 4 — Confirm Key is Token-Resident

### Command run:

```bash
pkcs11-tool --module /opt/homebrew/lib/softhsm/libsofthsm2.so \
--login --pin 1234 \
--list-objects
```

### Output:

```bash
Public Key Object: RSA 2048 bits
Label: CVI-IssCA-Key

Private Key Object: RSA 2048 bits
Label: CVI-IssCA-Key
Access: sensitive, always sensitive, never extractable
```

### What the output proves:

The output demonstrated that the cryptographic key pair exists inside the token itself and that the private key cannot be exported. This simulates how enterprise HSMs securely store CA private keys internally.

---

## Step 5 — Additional Steps Demonstrated

### Command:

```bash
softhsm2-util --show-slots
```

### What it showed:

The instructor demonstrated that the token retained its initialized state and continued securely storing the generated cryptographic keys.

---

# Part C — SoftHSM2 vs. a Physical HSM

**Physical HSM reference:** Thales Luna Network HSM

| Dimension | SoftHSM2 (Demo) | Thales Luna (Enterprise) |
|---|---|---|
| Key storage location | Software filesystem | Dedicated hardware appliance |
| Tamper resistance | None | Physical tamper-resistant hardware |
| PKCS#11 interface | Same | Same |
| FIPS validation | Not applicable | FIPS 140-2/140-3 validated |
| Used in production CAs | No — simulation only | Yes |
| Cost | Free / open source | Expensive enterprise hardware |
| Required for a CA private key | No | Common in enterprise PKI |

## What does a physical HSM protect against that SoftHSM2 cannot?

A physical HSM protects against physical theft, malware attacks, unauthorized key extraction, and operating system compromise. Enterprise HSMs contain tamper-resistant hardware protections that prevent attackers from exporting or copying private keys even if they gain administrative access to the operating system.

---

# Part D — The Key Ceremony Concept

## What is a key ceremony?

A key ceremony is a formal and highly controlled process used to generate and manage sensitive cryptographic keys. Enterprise PKI environments conduct key ceremonies with multiple administrators present to ensure accountability, separation of duties, and auditability. Each step is documented and monitored to reduce the risk of insider threats or unauthorized key access. Key ceremonies are especially important for Root CA and Issuing CA environments because those private keys establish trust for the entire PKI hierarchy.

## What would be different about this ceremony if CVI Issuing CA 1 were using a physical HSM?

If CVI Issuing CA 1 were using a physical HSM, the ceremony would take place in a secured data center or restricted environment with multiple administrators physically present. Hardware tokens, smart cards, quorum authentication, and physical access controls would likely be required. Formal audit documentation, witness signatures, and security logs would also be produced as part of the ceremony process.

---

# Part E — Connection to Phase 2

## Software-protected CA key vs. HSM-protected CA key

The operational risk of a software-stored CA private key is that an attacker who compromises the operating system could potentially steal or export the key material. An HSM-protected key significantly reduces this risk because the private key remains protected inside secure hardware and cannot be exported.

## When you back up the CA in Week 13, what specific step is more sensitive because the private key is software-protected?

When backing up the CA using `certutil -backup`, the exported backup files become highly sensitive because they contain the software-protected private key material. These backup files must be securely stored and protected from unauthorized access.

---

# Reflection

## The most important thing you took away from this demonstration:

The most important lesson from this demonstration was understanding how enterprise PKI environments protect highly sensitive CA private keys using token-resident storage and non-extractable key protections. I also learned how PKCS#11 standardizes communication between software and HSM devices.

## One question the demonstration raised that you want to understand better:

I would like to better understand how enterprise organizations securely perform HSM backup and disaster recovery procedures without exposing sensitive private key material.

---

# Submission Checklist

- [x] Part A: SoftHSM2 and PKCS#11 described in own words
- [x] Part B: All ceremony steps documented — commands, outputs, explanations
- [x] Part C: SoftHSM2 vs. Thales Luna comparison table completed
- [x] Part C: Physical HSM protection question answered
- [x] Part D: Key ceremony concept explained in own words
- [x] Part D: Enterprise ceremony differences described
- [x] Part E: Software key vs. HSM key risk addressed
- [x] Part E: Connection to Week 13 backup noted
- [x] Reflection completed
- [x] File saved as `lab-03-hsm-observation.md`
- [x] File committed to portfolio repo under `labs/week-10/`
