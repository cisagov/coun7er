#  Rotate Application Programming Interface (API) Keys

* **ID:** CM0083
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Application/cloud administration; API keys and secret rotation

## Intended Outcome

Rotating API keys disrupts privilege escalation, credential access, defense evasion, lateral movement, and persistence using stolen API keys.

## Introduction

An Application Programming Interface (API) is a system access point or library that has a well-defined syntax enabling software components to communicate by exchanging data in a structured and standardized way. Communication is made possible with an API key that is used to authenticate and authorize the application (or party) making the request. While API keys are not the only means of authenticating and authorizing requests, they are the simplest, and therefore very common across different use cases. 

Adversaries who obtain private API keys may use them to make fraudulent requests and exploit API functionality, enabling their attack campaigns. An adversary who steals a sensitive API key may alter or exfiltrate sensitive data in an organization, compromising the integrity of critical services such as file sharing, repositories, and collaboration platforms.

## Preparation

-  Work toward establishing a comprehensive view of the organization's API attack surface. This may include consulting documentation on API keys and access control records. 
- Review which applications/services use the affected API to understand what applications/services will be impacted. 
- Identify the attack path that allowed the adversary to steal the API key(s) and remediate. If malware was used to harvest the API keys or gain a foothold in the network to facilitate information gathering, purge the malware and fully evict the adversary from the environment to prevent them from re-harvesting the API keys. If the API key was inadvertently leaked publicly, consider taking down the source of the exposed key, if possible.
- Confirm the necessary permissions are made available to change the API key when rotated.

## Risks

- Rotating an API key may break communication between components that rely on stolen hardcoded keys or stolen keys stored on devices, risking downtime. 

## Guidance

Rotating API keys consists of three core steps: 

1. Generating a new API key
2. Replacing the compromised key
3. Revoking the compromised key and securely deleting the old API key

Note that different vendors and SaaS platforms have different processes for rotating their API keys. Organization's should consult vendor documentation for specific guidance relevant to their environment.

How To Rotate provides step-by-step instructions for rotating API keys for a variety of SaaS platforms (Link may be found in the references below.)


### Implement Additional Authentication Protections

Even after rotating API keys, multiple additional protections should be considered if appropriate. These include:

- Migrating to phishing resistant Multi-factor Authentication.
- Replacing API keys with token-based authentication, if appropriate.
- Ensuring API keys are regularly rotated. Typically it is recommended that keys are rotated at least every 90 days.

## Associated Techniques

- [T1528](https://attack.mitre.org/techniques/T1528/) -  Steal Application Access Token

## Related Countermeasures

- CM0050 - Reset Kerberos Ticket Granting Ticket (KRBTGT) Password Twice
- CM0028 - Reset Service Account Passwords
- CM0030 - Remove Known Malware
- CM0075 - Implement Access Restrictions on Cloud Storage Objects
- CM0076 - Remove Adversary Certificates and Rotate Secrets for Applications and Service Principals
- CM0077 - Revoke Microsoft 365 Refresh Tokens
- CM0080 - Block and Reissue Tokens
- CM0143 - Audit and Configure PI Web API

## References

- Getting Started | <https://howtorotate.com/docs/introduction/getting-started/>
- How to Become Great at API Key Rotation: Best Practices and Tips | <https://securityboulevard.com/2023/12/how-to-become-great-at-api-key-rotation-best-practices-and-tips/>
- Mapping the MITRE ATT&CK Framework to API Security | <https://salt.security/blog/mapping-the-mitre-att-ck-framework-to-api-security>
