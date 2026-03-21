# Lab 04 — Detect Certificate Misconfigurations

## Overview
This lab analyzes common certificate misconfigurations and how they break TLS trust.

---

## Scenario 1 — Missing Subject Alternative Name

**Would modern browsers trust this certificate?**  
No.

**Analysis:**  
Browsers require SAN to match the hostname. CN alone is not enough.

---

## Scenario 2 — Incorrect Extended Key Usage

**Would a browser accept this certificate for a web server?**  
No.

**Analysis:**  
It must include TLS We# Lab 04 — Detect Certificate Misconfigurations

## Overview
This lab focused on analyzing common certificate misconfigurations that can break TLS validation and cause outages. The PKI concept being investigated was how certificate fields, extensions, validity periods, and trust chains affect whether a browser will trust a certificate.

---

## Scenario 1 — Missing Subject Alternative Name

**Would modern browsers trust this certificate?**  
No. Modern browsers would not trust this certificate for `example.com`.

**Analysis:**  
Modern browsers require the Subject Alternative Name (SAN) extension to identify which hostname a certificate is valid for. The Common Name (CN) alone is no longer considered sufficient for hostname validation because browsers and standards now rely on SAN as the authoritative source. A user would likely see a certificate warning or privacy error indicatin
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

# Lab 04 — Detect Certificate Misconfigurations

## Overview
This lab analyzes common certificate misconfigurations and how they break TLS trust.

---

## Scenario 1 — Missing Subject Alternative Name

**Would modern browsers trust this certificate?**  
No.

**Analysis:**  
Browsers require SAN to match the hostname. CN alone is not enough.

---

## Scenario 2 — Incorrect Extended Key Usage

**Would a browser accept this certificate for a web server?**  
No.

**Analysis:**  
It must include TLS Web Server Authentication. Client Authentication alone will fail.

---

## Scenario 3 — Expired Certificate

**What happens if this certificate is used today?**  
Validation fails.

**Analysis:**  
The certificate is expired and no longer trusted.

---

## Scenario 4 — Missing Intermediate Certificate

**What error would a browser likely display?**  
Chain error / cannot verify issuer.

**Analysis:**  
The browser cannot build a trust chain to a root CA.

---

## Key Takeaway
Misconfigurations like missing SAN, wrong EKU, expiration, or missing intermediates break trust even if encryption is correct.
