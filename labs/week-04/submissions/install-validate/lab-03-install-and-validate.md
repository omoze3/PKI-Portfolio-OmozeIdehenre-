# Lab 03 — Install a Certificate and Validate Trust (Stretch)

## Overview
This lab explored how trust is established and controlled within Public Key Infrastructure (PKI). The objective was to generate a self-signed root Certificate Authority (CA), install it into the system trust store, use it to sign another certificate, and observe how trust is validated and removed.

This demonstrated how operating systems determine whether a certificate is trusted based on the presence of a trusted root CA.

---

## Environment
- Operating System: macOS
- Terminal Used: Mac Terminal
- OpenSSL Version (`openssl version`): 3.3.6
---

## Steps Performed
1. Generated a private key and created a self-signed root CA certificate using OpenSSL
2. Verified the root CA certificate and confirmed it was self-signed
3. Installed the root CA into the macOS system keychain as a trusted certificate
4. Generated a private key and CSR for a test certificate
5. Signed the test certificate using the root CA
6. Verified the signed certificate against the root CA (successful validation)
7. Removed the root CA from the system trust store
8. Verified the certificate again to confirm trust failure

---

## Results

- The root CA certificate showed identical Subject and Issuer fields, confirming it was self-signed

- Before removal:
  - Verification output:
    ```
    test-signed.crt: OK
    ```
  - This confirmed the system trusted the certificate chain

- After removal:
  - Verification output:
    ```
    verification failed: 20 (unable to get local issuer certificate)
    ```
  - This confirmed the trust chain was broken after removing the root CA

- Trust chain validation was successfully established when the root CA was installed and failed immediately after removal

---

## Key Findings
- A root CA acts as the foundation of trust in PKI systems
- Installing a root CA into the system trust store allows all certificates signed by it to be trusted
- Removing a root CA immediately invalidates all certificates that depend on it for trust

---

## Explanation

The test root CA was self-signed, meaning its Subject and Issuer fields were identical. This indicates that it is the top-level authority in the trust chain and does not rely on any higher authority.

When the root CA was installed into the system trust store, the operating system recognized it as a trusted authority. As a result, any certificate signed by that root CA was automatically trusted.

In enterprise environments, system administrators and security teams control which root CAs are installed on employee machines. This allows organizations to enforce internal trust policies and inspect secure traffic when necessary.

If an attacker is able to install a malicious root CA on a system, they can intercept and decrypt secure communications (man-in-the-middle attacks), making this a critical security risk.

---

## Challenges / Troubleshooting

There was initial confusion when verifying certificate trust because using the `-CAfile` flag manually provides trust, bypassing the system trust store. This was resolved by running verification without specifying a CA file to correctly test system-level trust.

Additionally, removing the certificate via CLI required troubleshooting due to macOS keychain behavior, and verification results were used to confirm successful removal.

---

## Artifacts
- test-root-ca.crt
- test-signed.crt
- test-signed.csr

