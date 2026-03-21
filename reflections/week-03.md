# Week 03 Reflection

## 1. What did you learn this week?
This week I learned how X.509 certificates work in real-world PKI systems. I practiced retrieving certificates from live websites, reading their fields using OpenSSL, and understanding how identity, encryption, and trust are established through certificates. I also learned how certificate extensions like SAN, Key Usage, and Extended Key Usage control how a certificate can be used. Additionally, I gained hands-on experience verifying certificate chains and identifying common misconfigurations that can break secure connections.

---

## 2. What concept was most challenging?
The most challenging concept was understanding certificate chains and how trust is built from the leaf certificate up to the root CA. It took time to fully grasp how the Issuer and Subject fields connect each certificate and why intermediate certificates are required.

---

## 3. Where does this concept appear in real-world systems?
These concepts appear everywhere HTTPS is used. Every secure website relies on certificates and PKI to establish trust between the browser and the server. This is also used in APIs, software updates, and enterprise systems.

---

## 4. How would you explain this topic to a non-technical audience?
Certificates are like digital ID cards for websites. They prove that a website is real and safe to connect to, while encryption keeps your information private.

---

## 5. What questions remain?
I want to better understand how Certificate Authorities are trusted globally and how certificate revocation works in real-world systems.

---

## Professional Growth Check

- [x] I documented my work clearly and in my own words
- [x] I used structured formatting in my submission files
- [x] My commit message was meaningful and descriptive
