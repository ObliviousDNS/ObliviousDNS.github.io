---
layout: default
title: Oblivious DNS
nav_order: 1
has_children: false
permalink: /
---

# Overview

It is well known that DNS leaks information that an Internet user may want to keep private, such as the websites she is visiting, user identifiers, MAC addresses, and the subnet in which she is located. This information can be visible to a 3rd party eavesdropping on the communication between a client and a recursive resolver, or even between a recursive resolver and an authoritative server. As this information is sent to each DNS server, DNS operators can also see clients' information. 

![TEST](./_site/assets/images/overview.png)

While there has been some previous work on increasing privacy in DNS infrastructure, such as DNS Query Name Minimization and DNS-Over-TLS, these approaches do not fully solve the problem. Both of these are steps in the right direction, but neither prevent DNS operators from learning information which domains specific users are interested in. Our work is concerned with a powerful adversary that has the capabilities to: 1) eavesdrop on communications between clients and recursive resolvers, and between recursive resolvers and authoritative name servers, 2) request data (via subpoena/warrant) from any number of DNS operators, 3) maliciously access data at any DNS server. 

To address this type of attacker, we present Oblivious DNS (ODNS), which is a new design of the DNS ecosystem that allows current DNS servers to remain unchanged and increases privacy for data in motion and at rest. In the ODNS system, both the client is modified with a local resolver, and there is a new authoritative name server for .odns. To prevent an eavesdropper from learning information, the DNS query must be encrypted; the client generates a request for www.foo.com, generates a session key k, encrypts the requested domain, and appends the TLD domain .odns, resulting in {www.foo.com}k.odns. The client forwards this, with the session key encrypted under the .odns authoritative server's public key ({k}PK) in the "Additional Information" record of the DNS query to the recursive resolver, which then forwards it to the authoritative name server for .odns. The authoritative server decrypts the session key with his private key, and then subsequently decrypts the requested domain with the session key. The authoritative server then forwards the DNS request to the appropriate name server, acting as a recursive resolver. While the name servers see incoming DNS requests, they do not know which clients they are coming from; additionally, an eavesdropper cannot connect a client with her corresponding DNS queries. 

![TEST](./_site/assets/images/ODNSoverview.png)

As this is ongoing work, we have some future work to continue in this direction. We have implemented a prototype of ODNS to evaluate its feasibility and to measure its performance overhead in comparison to current DNS performance. 

# Resources

Papers:
- Schmitt, Paul, et al. "Oblivious DNS: Practical privacy for DNS queries." Proceedings on Privacy Enhancing Technologies 2019.2 (2019): 228-244.
Talks.
- DNS-OARC 28, San Juan, Puerto Rico, March 2018.

# Contact
We are a team of researchers from the University of Chicago and the University of Hawaii. Feel free to contact us if you have any questions. 
- Paul Schmitt
- Nick Feamster
- Annie Edmundson