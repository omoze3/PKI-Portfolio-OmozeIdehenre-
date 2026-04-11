Week 01 Reflection

1. What did you learn this week?

This week, I learned the foundational concepts of key pairing and trust chains within Public Key Infrastructure (PKI). I now understand how a public key and private key work together to secure communication, and how certificates are used to establish identity. I also learned how trust is not assumed, but built through a chain of trusted Certificate Authorities (CAs) that validate and vouch for systems.

2. What concept was most challenging?

The most challenging concept was understanding the trust chain. It took time to fully grasp how trust is passed from a root Certificate Authority down through intermediate certificates to the end-entity certificate. Visualizing how each link in the chain must be valid for the entire system to be trusted required deeper thinking.

3. Where does this concept appear in real-world systems?

These concepts appear in everyday systems such as HTTPS websites, web browsers, and cloud platforms. For example, when visiting a secure website, the browser verifies the site’s certificate by checking its trust chain back to a trusted root CA. This process ensures that users are connecting to legitimate and secure services.

4. How would you explain this topic to a non-technical audience?

I would explain it as a system of digital identity and trust. Imagine sending a locked box (encrypted message) that only the intended person can open (private key). To make sure you’re sending it to the right person, you check their official ID (certificate), which has been verified by a trusted authority. The trust chain is like a series of verified endorsements that confirm the identity is legitimate.

5. What questions remain?

At this time, I do not have any major questions. However, I am interested in learning more about how trust chains are managed and maintained in large enterprise environments.

Professional Growth Check
I documented my work clearly and in my own words
I used structured formatting in my submission files
My commit message was meaningful and descriptive
