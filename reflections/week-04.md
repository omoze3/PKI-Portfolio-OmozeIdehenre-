# Week 04 Reflection

## 1. What did you learn this week?

This week I learned how Public Key Infrastructure (PKI) works in practice, not just conceptually. I worked hands-on with certificate formats (PEM, DER, PFX), explored how operating systems manage trusted root Certificate Authorities (CAs), and built my own trust chain by creating and installing a root CA.

The most impactful learning was understanding how trust is established and broken. I saw firsthand how installing a root CA allows a system to trust certificates, and how removing it immediately invalidates that trust.

---

## 2. What concept was most challenging?

The most challenging concept was understanding the difference between manual trust and system trust.

At first, I was confused because certificate verification kept returning “OK,” even after removing the root CA. I learned that using the `-CAfile` flag in OpenSSL bypasses the system trust store and manually provides trust.

Once I removed that flag and verified using the system trust store, I clearly saw how trust failed after removing the root CA.

---

## 3. Where does this concept appear in real-world systems?

These concepts are used everywhere in real-world systems:

- HTTPS connections in browsers rely on trusted root CAs to validate websites
- Enterprise environments install internal root CAs to inspect or secure traffic
- Software signing ensures applications are trusted before execution
- Cloud platforms and APIs use certificates for secure communication
- Security tools rely on certificate validation to prevent man-in-the-middle attacks

Understanding PKI is critical for cybersecurity, cloud security, and secure system design.

---

## 4. How would you explain this topic to a non-technical audience?

I would explain PKI like a system of trusted IDs.

A root CA is like a trusted authority (similar to a government issuing IDs). If that authority vouches for someone, you trust them.

When your computer sees a secure website, it checks:
“Was this certificate issued by someone I trust?”

If yes → connection is safe  
If no → warning or failure  

If a bad actor installs their own “fake authority” on your system, they can trick your computer into trusting malicious connections.

---

## 5. What questions remain?

- How do organizations securely manage and rotate root CAs at scale?
- How do browsers decide which root CAs to trust globally?
- What are best practices for protecting private keys in enterprise environments?
- How does certificate revocation (CRL/OCSP) work in real-world systems?
- How do Zero Trust architectures extend or replace traditional PKI models?

---

## Professional Growth Check

- [x] I documented my work clearly and in my own words
- [x] I used structured formatting in my submission files
- [x] My commit message was meaningful and descriptive
