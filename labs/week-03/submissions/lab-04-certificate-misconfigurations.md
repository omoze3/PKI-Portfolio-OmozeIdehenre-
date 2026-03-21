# Lab 04 — Detect Certificate Misconfigurations

## Overview
This lab focused on analyzing common certificate misconfigurations that can break TLS validation and cause outages. The PKI concept being investigated was how certificate fields, extensions, validity periods, and trust chains affect whether a browser will trust a certificate.

---

## Scenario 1 — Missing Subject Alternative Name

**Would modern browsers trust this certificate?**  
No. Modern browsers would not trust this certificate for `example.com`.

**Analysis:**  
Modern browsers require the Subject Alternative Name (SAN) extension to identify which hostname a certificate is valid for. The Common Name (CN) alone is no longer considered sufficient for hostname validation because browsers and standards now rely on SAN as the authoritative source. A user would likely see a certificate warning or privacy error indicating that the certificate is not valid for the requested hostname.

---

## Scenario 2 — Incorrect Extended Key Usage

**Would a browser accept this certificate for a web server?**  
No. A browser would not accept this certificate for HTTPS on a web server.

**Analysis:**  
Extended Key Usage (EKU) defines what purposes a certificate is allowed to be used for. A certificate used for HTTPS must include `TLS Web Server Authentication` so the browser knows it is valid for server-side authentication. If the certificate only contains `Client Authentication`, the browser would reject it for web server use and the user would likely see a certificate error or secure connection failure.

---

## Scenario 3 — Expired Certificate

**What happens if this certificate is used today?**  
TLS validation would fail, and the browser would reject the connection.

**Analysis:**  
Certificates are only valid during the time window between the `Not Before` and `Not After` dates. Once the current date is past the expiration date, the certificate is no longer trusted because its validity period has ended. Certificate lifecycle management is critical because organizations must renew and replace certificates before they expire or they risk outages. A user would likely see a browser warning such as “Your connection is not private” or an error stating the certificate has expired.

---

## Scenario 4 — Missing Intermediate Certificate

**What error would a browser likely display?**  
The browser would likely display an error indicating that the certificate chain is incomplete or that the issuer could not be verified.

**Analysis:**  
Certificate chains establish trust by linking the server certificate to an intermediate CA and then to a trusted root CA. If the server does not provide the intermediate certificate, the browser may not be able to build a full chain back to a trusted root, so validation fails. Servers must include the intermediate certificate because clients cannot always retrieve it automatically. Some browsers handle this differently than others because some cache intermediates from prior visits or fetch missing certificates dynamically, while others require the full chain to be presented by the server.

---

## Key Takeaway
Certificate misconfiguration is one of the most common causes of PKI outages because trust depends on more than just having a certificate. Even when cryptography is correct, missing SAN entries, wrong EKU values, expired certificates, or incomplete chains can cause browsers and clients to reject secure connections.
