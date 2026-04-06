# Certificate Chain (Lab 03)

This folder contains the TLS certificate chain extracted and organized in trust order:

1. 01-root.pem  
   - Root Certificate Authority (self-signed trust anchor)

2. 02-intermediate.pem  
   - Intermediate CA that signs the server certificate

3. 03-server.pem  
   - Leaf certificate presented by the server

## Notes

- Certificates are ordered to reflect real-world trust validation flow
- Root certificate is typically stored in system trust stores
- This chain was extracted using OpenSSL with SNI enabled
