# Prevent and Detect Network Traffic to/from The Onion Router (Tor) Exit Nodes

* **ID:** CM0093
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Network Operations (IO-WRL-004)
* **Skills & Knowledge:** Network security; Tor exit-node intelligence and traffic filtering

## Intended Outcome

Preventing and detecting network traffic to/from The Onion Router (Tor) exit nodes protects against attempts at reconnaissance, initial access, command-and-control (C2), exfiltration, and impact originating from, or destined to the Tor network.

## Introduction

Tor is software that encrypts and proxies traffic through multiple nodes to provide users a degree of anonymity.  While there are legitimate use-cases for Tor, adversaries commonly use the software to anonymize their attempts to perform reconnaissance, initial access, command-and-control, exfiltration, and adversely impact targets.  

## Preparation

-	Assess existing security tools for the ability to protect against and detect activity consistent with Tor usage.
-	Maintain an updated list of Tor exit nodes.  Tor provides lists of exit nodes available in multiple formats (see references). 

## Risks

There are legitimate use cases for Tor.  The countermeasures recommended herein are restrictive given the contextual assumption of incident response.  Be aware that these countermeasures risk disrupting non-malicious Tor traffic.  If this is a concern, you may opt to monitor suspected Tor traffic prior to deciding on a responsive course of action.  Additionally, it is important to note that not all Tor exit nodes are publicly listed.  As such, this countermeasure does not eliminate the threat of adversary activity originating form or destined for Tor nodes.   

## Guidance

### Prevent

-	Configure external network security tools to block all inbound network traffic from Tor exit nodes.  The intent is to prevent or respond to reconnaissance, initial access, and attempts to disrupt operations originating from known Tor exit nodes. 
-	Configure internal network security tools to block all outbound network traffic to Tor exit nodes.  The intent is to prevent or respond to command-and-control and/or exfiltration from internal assets to Tor exit nodes.  

### Detect

-	Monitor TCP/UDP ports 9001, 9030, 9040, 9050, 9051, 9150 and TCP 443, 8443. Monitoring TCP 443 will generate a high rate of false positives and will require fine-tuning and correlative analysis. 
-	Monitor DNS queries for domain names ending in torproject.org.
-	Monitor DNS queries for domains ending in. onion to detect misconfigured Tor clients.
-	Monitor for unusually large dataflows or persistent connections between hosts on the network and Tor exit nodes to detect command-and-control or exfiltration. 

## Associated Techniques

- [T1594](https://attack.mitre.org/techniques/T1594/) - Search Victim-Owned Websites
- [T1595.001](https://attack.mitre.org/techniques/T1595/001/) - Active Scanning: Scanning IP Blocks
- [T1595.002](https://attack.mitre.org/techniques/T1595/002/) - Active Scanning: Vulnerability Scanning
- [T1595.003](https://attack.mitre.org/techniques/T1595/003/) - Active Scanning: Wordlist Scanning
- [T1598.002](https://attack.mitre.org/techniques/T1598/002/) - Phishing for Information: Spearphishing Attachment
- [T1598.003](https://attack.mitre.org/techniques/T1598/003/) - Phishing for Information: Spearphishing Link
- [T1583.005](https://attack.mitre.org/techniques/T1583.005/) - Acquire Infrastructure: Botnet
- [T1659](https://attack.mitre.org/techniques/T1659/) - Content Injection
- [T1190](https://attack.mitre.org/techniques/T1190/) - Exploit Public-Facing Application
- [T1133](https://attack.mitre.org/techniques/T1133/) - External Remote Services
- [T1566.001](https://attack.mitre.org/techniques/T1566/001/) - Phishing: Spearphishing Attachment
- [T1566.002](https://attack.mitre.org/techniques/T1566/002/) - Phishing: Spearphishing Link
- [T1078.001](https://attack.mitre.org/techniques/T1078/001/) - Valid Accounts: Default Accounts
- [T1078.004](https://attack.mitre.org/techniques/T1078/004/) - Valid Accounts: Cloud Accounts
- [T1090.001](https://attack.mitre.org/techniques/T1090/001/) - Proxy: Internal Proxy
- [T1090.003](https://attack.mitre.org/techniques/T1090/003/) - Proxy: Multi-hop Proxy
- [T1090.004](https://attack.mitre.org/techniques/T1090/004/) - Proxy: Domain Fronting
- [T1573.002](https://attack.mitre.org/techniques/T1573/002/) - Encrypted Channel: Asymmetric Cryptography
- [T1105](https://attack.mitre.org/techniques/T1105/) - Ingress Tool Transfer
- [T1048.002](https://attack.mitre.org/techniques/T1048.002/) - Exfiltration Over Alternative Protocol: Exfiltration Over Asymmetric Encrypted Non-C2 Protocol
- [T1041](https://attack.mitre.org/techniques/T1041/) - Exfiltration Over C2 Channel 
- [T1567](https://attack.mitre.org/techniques/T1567/) - Exfiltration Over Web Service
- [T1491](https://attack.mitre.org/techniques/T1491/) - Defacement
- [T1499](https://attack.mitre.org/techniques/T1499/) - Endpoint Denial of Service
- [T1498](https://attack.mitre.org/techniques/T1498/) - Network Denial of Service

## Related Countermeasures

-	CM0011 – Enable Domain Name System (DNS) Sinkhole
-   CM0009 - Update Domain Name Service (DNS) Deny List
-   CM0010 - Enable Internet Protocol (IP) Address Allowlists
- CM0106 - Eliminate Web Shells

## References

- Defending Against Malicious Cyber Activity Originating from Tor | <https://www.cisa.gov/news-events/cybersecurity-advisories/aa20-183a>
- Changes to the Tor Exit List Service | <https://blog.torproject.org/changes-tor-exit-list-service/>
