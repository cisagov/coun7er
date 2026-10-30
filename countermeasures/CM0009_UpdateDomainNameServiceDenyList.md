# Update Domain Name Service (DNS) Deny List

* **ID:** CM0009
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Network Operations (IO-WRL-004)
* **Skills & Knowledge:** Network administration; DNS filtering and deny lists

## Intended Outcome

Updating the Domain Name Service (DNS) deny list blocks adversary command and control (C2). 

## Introduction

Adversaries who acquire infrastructure can use bad domains to run C2-based offensive operations. 

## Preparation

No Preparation content identified. 

## Risks

- Blocking domains can unintentionally prevent access to domains that that are needed for the enterprise.

## Guidance 

DNS blacklists or deny lists are often to filter and block emails containing known bad domains. They can also be used to block, blocks of IP addresses or even an internet service provider known for spam. There are two ways to block a domain: 
- Domain redirect - A domain redirect will redirect a flagged domain to a quarantine zone.
- Request denied - A request denied will refuse DNS queries from flagged domains. 

## Associated Techniques

- [T1583.001](https://attack.mitre.org/techniques/T1583/001/) - Acquire Infrastructure: Domains
- [T1583.002](https://attack.mitre.org/techniques/T1583/002/) - Acquire Infrastructure: DNS Server
- [T1566](https://attack.mitre.org/techniques/T1566/) - Phishing

## Related Countermeasures

- CM0011 - Enable Domain Name System (DNS) Sinkhole
- RM0106 - Enforce enterprise DNS resolution (*future*)
- RM0262 - Enforce enterprise DNS resolution for all systems (*future*)
- RM0271 - Implement or configure local DNS trap (*future*)
- RM0369 - Isolate affected systems by blocking specific external DNS domains, as listed by CISA18 (*future*)
- CM0035 - Configure Uniform Resource Locator (URL) Filtering
- CM0040 - Clear Workstation and Server Domain Name Server (DNS) Caches
- CM0093 - Prevent and Detect Network Traffic to/from The Onion Router (Tor) Exit Nodes

## References 

- What is a DNSBL? | <https://whatismyipaddress.com/dnsbl-blacklist>
- DNS Blocking: A Viable Strategy in Malware Defense | <https://insights.sei.cmu.edu/blog/dns-blocking-a-viable-strategy-in-malware-defense/>
