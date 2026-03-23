# Lab 04 — Detect Certificate Misconfigurations

## Overview
This lab focused on identifying common certificate misconfigurations that cause TLS validation failures in real-world systems. The goal was to understand how specific certificate fields and extensions impact whether a browser trusts a connection. This directly relates to PKI trust chains, certificate validation, and secure HTTPS communication.

---

## Scenario 1 — Missing Subject Alternative Name

**Would modern browsers trust this certificate?**  
No. Modern browsers would not trust this certificate for `example.com`.

**Analysis:**  
Modern browsers require the Subject Alternative Name (SAN) extension to validate the hostname of a website. The Common Name (CN) is no longer used for hostname verification because it is considered outdated and less flexible. If SAN is missing, the browser cannot confirm that the certificate is valid for the requested domain. A user would typically see a browser error such as “Your connection is not private” or a hostname mismatch warning.

---

## Scenario 2 — Incorrect Extended Key Usage

**Would a browser accept this certificate for a web server?**  Extended Key Usage: Client Authentication
No. A browser would reject this certificate for HTTPS.

**Analysis:**  
Extended Key Usage (EKU) defines what a certificate is allowed to be used for. For HTTPS, the certificate must include `TLS Web Server Authentication`. If the certificate only includes `Client Authentication`, it is not valid for server identity. Because of this mismatch, the browser will fail validation and block the connection, often showing a certificate usage error or secure connection failure.

---

## Scenario 3 — Expired Certificate

**What happens if this certificate is used today?**  
TLS validation fails, and the browser will reject the connection.

**Analysis:**  
Certificates are only trusted within their validity period, defined by the “Not Before” and “Not After” fields. Once the current date passes the expiration date, the certificate is considered invalid and untrusted. This is why certificate lifecycle management is critical in production environments. If a certificate expires, users will see warnings such as “Certificate expired” or “Your connection is not private,” which can lead to service outages.

---

## Scenario 4 — Missing Intermediate Certificate

**What error would a browser likely display?**  
The browser would show a trust or chain validation error, such as “Unable to verify issuer” or “Incomplete certificate chain.”

**Analysis:**  
Certificate trust is established through a chain from the leaf certificate to an intermediate CA and finally to a trusted root CA. If the server does not provide the intermediate certificate, the browser cannot build this chain and therefore cannot verify trust. Some browsers may attempt to fetch missing intermediates automatically, but others will fail outright. This inconsistency can cause intermittent issues across different environments.

---

## Key Takeaway
Certificate misconfigurations are one of the most common causes of TLS failures in real-world systems. Even when encryption is functioning correctly, missing SAN fields, incorrect EKU values, expired certificates, or incomplete chains will break trust and cause browsers to reject connections. Proper certificate configuration and lifecycle management are critical to maintaining secure and reliable systems.
