# Lab — Symmetric Encryption (Confidentiality)

## Goal

This lab builds operational understanding of symmetric encryption and the security property of confidentiality.

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
