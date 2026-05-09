# Week 09 Reflection

## What did you learn this week?

This week, I learned how to validate and document a functioning Active Directory Certificate Services (AD CS) environment. I verified connectivity between systems, confirmed that the Certification Authority service was running, and explored how certificate infrastructure integrates with Active Directory. I also learned how certificate templates, CRL distribution points, AIA paths, and CA hierarchy settings work together to support trust and certificate validation in an enterprise PKI environment.

In addition, I gained more hands-on experience using PowerShell tools such as `certutil`, `Test-Connection`, and `Get-Service` to inspect and validate PKI infrastructure components.

---

## What concept was most challenging?

The most challenging concept was understanding how the different PKI components connect together inside Active Directory. It took time to fully understand the relationship between the Root CA, Issuing CA, certificate templates, CRLs, AIA paths, and AD replication. There are many moving parts, and each one plays a role in maintaining trust and certificate validation across the environment.

Another challenge was navigating Windows administrative tools inside a Mac-hosted VM environment while documenting everything accurately in markdown format.

---

## Where does this concept appear in real-world systems?

These concepts appear throughout enterprise environments that rely on secure authentication and encrypted communications. Examples include:

- HTTPS websites and internal web applications
- Smart card and user authentication systems
- VPN authentication
- Domain Controller authentication
- Enterprise Wi-Fi authentication
- Secure email and file encryption
- Certificate-based authentication in cloud and hybrid infrastructure

Organizations use Active Directory Certificate Services to centrally manage trust, certificate issuance, and revocation across systems and users.

---

## How would you explain this topic to a non-technical audience?

I would explain PKI and certificate services like a trusted identification system. The Root CA acts like the highest trusted authority, while the Issuing CA creates and distributes trusted digital IDs to users, computers, and services. These digital certificates prove that systems are legitimate and safe to communicate with.

Certificate revocation lists work like a “do not trust” list for expired or compromised identities, while Active Directory helps distribute trust information automatically across the organization.

---

## What questions remain?

- How are CRLs and OCSP managed at scale in large enterprise environments?
- What monitoring and alerting tools are commonly used for PKI health and certificate expiration?
- How do organizations securely rotate and protect Root CA private keys?
- How does certificate lifecycle automation integrate with cloud-native systems and DevOps workflows?

