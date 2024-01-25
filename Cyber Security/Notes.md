---
tags: cyber-sec
deck: cyber-security
---

# Fundamental set of questions to ask in cyber security #card
<!-- 1701338141149 898b0f3142766506385178cb969c299b -->

- What is being protected?
- Are there any known threats and vulnerabilities?
- What are the impacts to the organization if the data is lost or leaked?
- What is the value of the data to the organization?
- What can be done to mitigate the risks?

# The most commonly-used terms in cyber security lingo #card
<!-- 1701338141187 19c648e7af473bc464f084311bcc15a3 -->

Asset, threat, vulnerability and exploit.

- assert is what is being protected, something that has some value to its owner. It can be tangible (gold or a running server) or intangible (data).
- threat is an intention to cause damage. For cyber security this can be defined as a hostile act aimed by an attacker at an asset. Regardless of the attacker's intent to do no harm, a threat is still a threat. The attacker posing a threat is called a threat actor.
- vulnerability is  defect in a system. This defect may be a bug in the application code or flaw in the design of the system. Vulnerabilities can also be a consequences of improper configuration or user action.
- exploit is a way to take advantage of a known vulnerability. The usual objective is to take control over the asset.

# If a company would have its servers breached, what would be the possible reasons for the company not wanting to disclose the breach? #card
<!-- 1701338141221 f9ecaa307eaf475cb18c1bbff0b6d10e -->

1. Investigation in progress: company may be actively investigating the breach to understand the extent of the damage, identify the attackers, and gather evidence. Premature disclosure could compromise the effort and may lead to legal and regulatory issues.
2. Impact on the stock prices.
3. Reputation management and customer trust.
4. Lack of awareness.
5. Fear of further attacks.

# What is STRIDE threat model? #card
<!-- 1701338141237 088c577e87793c8f807a050774b50fe5 -->

It is a useful checklist of questions that can help in the threat-modelling of the application.

- Spoofing: covers cases where someone is illegally accessing a system using another user's authentication information.
- Tampering: refers to unauthorized modification of data or systems (config files, data in database, changing executable files, manipulating the content of messages in transit).
- Repudiation: specifies that a system should be able to trace user operations to provide evidence of what has happened in case of a breach.
- Information Disclosure: covers the exposure of information to unauthorized individuals.
- Denial of Service: refers to cases where the server or service is made temporarily unavailable.
- Elevation of Privilege: threat type in which an unprivileged user finds a way to gain sufficient privileges to compromise the system.

# What is DREAD risk assessment model? #card
<!-- 1701338141269 c94e34e547d5c2a5f3ac5e317884c259 -->

Is a checklist for prioritizing threats based on their severity.

- Damage
- Reproducibility
- Exploitability
- Affected Users
- Discoverability

A scale from 0-10 is usually used in all categories (discoverability is set to 10)

# How can threat actor use botnets? #card
<!-- 1701338141288 3b64e82808ecc03089579f1eabd417ba -->

- DDoS
- Spam and Phishing Campaigns
- Brute Force Attacks
- Cryptojacking
- Proxy Networks for Anonymity
