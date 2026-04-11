Week 06 Reflection

PKI Incident Diagnosis & Troubleshooting

What did you learn this week?

This week, I learned how to think like a PKI engineer by applying a structured diagnostic framework to real TLS failures. Instead of relying on memorized error messages, I worked through a repeatable 4-step process: retrieve the certificate, parse key fields, validate the chain, and check revocation and trust.

Each lab reinforced a different failure type. I diagnosed an expired certificate by analyzing the Not After date, identified a broken chain through missing intermediate certificates, and confirmed a SAN mismatch by comparing the hostname to the certificate’s Subject Alternative Name entries. By the end of the week, I was able to distinguish between certificate-level issues and trust/chain-level issues with confidence.

What concept was most challenging?

The most challenging concept was understanding certificate chain validation and how trust is established across multiple certificates. Specifically, recognizing why a certificate that looks valid on its own can still fail if the intermediate certificate is missing was not immediately intuitive.

It required slowing down and understanding how clients build trust paths from the leaf certificate up to a trusted root CA. The error “unable to get local issuer certificate” helped reinforce that the issue is not always the leaf certificate itself, but often the chain that supports it.

Where does this concept appear in real-world systems?

These concepts appear constantly in real-world environments, especially in enterprise systems, cloud infrastructure, and internal applications. For example:

Web applications fail when certificates expire or are misconfigured
Internal services break when intermediate certificates are not properly deployed
Load balancers and reverse proxies frequently introduce chain issues
Microservices and APIs rely heavily on TLS for secure communication

In large organizations, these failures can impact entire user populations, disrupt business operations, and create security risks if not resolved quickly.

How would you explain this topic to a non-technical audience?

I would explain PKI troubleshooting like checking identification at a secure building.

A certificate is like an ID badge. If the badge is expired, you are denied access. If the badge is valid but issued by an unknown authority, security still won’t trust it. And if the badge doesn’t match who you claim to be, access is denied.

This week focused on figuring out why access is denied — whether it’s because the badge is expired, doesn’t match the person, or can’t be verified by security.

What questions remain?

I want to better understand how these diagnostics scale in automated environments, especially in cloud-native systems where certificates are frequently rotated. I am also curious about how monitoring tools detect and alert on certificate issues before they impact users.

Additionally, I want to explore how organizations manage trust stores across distributed systems and how misconfigurations at that level can create widespread failures.

Professional Growth Check

- I documented my work clearly and in my own words
- I used structured formatting in my submission files
- My commit message was meaningful and descriptive

CVI PKI Career Pathway — Foundations Phase
