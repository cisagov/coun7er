# Isolate Endpoints from Network

* **ID:** CM0065
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Incident Response (PD-WRL-003)
* **Skills & Knowledge:** Endpoint/network administration; host isolation and containment

## Intended Outcome

Isolating an endpoint from the network blocks adversary initial access, lateral movement, and command and control to and from the host on a network.

## Introduction

An endpoint is any physical or virtual device or node on a network which is the ultimate destination of communications encompassing laptops, desktops, workstations, smartphones, servers, IoT devices, virtual machines, and other computing devices linked within an enterprise environment. 

Adversaries that compromise an endpoint can perform a range of techniques to accomplish their objectives. By isolating a compromised endpoint, responders will be able to contain an incident by preventing the threat from issuing C2 commands and/or moving laterally across a network.  Endpoint isolation can further disrupt or otherwise limit additional network-centric activities ranging from discovery, collection, and exfiltration - although automated actions that do not require real-time communication with the adversary may still occur.
  

## Preparation

-	Ensure plans, procedures, and authorities are in place to enable rapid containment of compromised endpoints. 
-	This countermeasure requires effective asset management and assumes use of endpoint management software or endpoint detection and response (EDR).
-	Having backups or redundant devices available to replace the compromised endpoint will mitigate the likelihood of operational interruption.

## Risks

- Isolating critical endpoints may disrupt important business operations. Responders should factor in potential operational impacts into their decision-making process.

## Guidance

Software solutions (e.g. CrowdStrike, Defender for Endpoint, SentinalOne, etc.) may be used to quickly segregate endpoints from the rest of the network. Alternately, teams may configure endpoint solutions to automatically isolate devices via workflows or playbooks under circumstances that warrant immediate containment. 

In some cases, system administrators may want to consider either logically segregating virtual separation or physical disconnection from the network.

While the steps to perform endpoint isolation vary depending on the circumstances of the incident, the characteristics of the network, and the options available to the organization, the following general steps are as follow:

-	Identify compromised endpoint(s)
-	Understand the compromised endpoint(s) purpose to assess the potential for operational impact
    -   Identify the type of endpoint (mobile device, workstation, server, etc.)
    -   Determine the endpoint’s function (desktop, web server, application server, etc.)
    -   Identify to whom the endpoint is assigned and the individual’s role (business function, administrator, c-suite/leadership, etc.)
-	Isolate the endpoint(s) from the network
    - As part of isolation, responders may freeze system processes and prevent user interaction and perform a forensic capture of the device.    
-	Investigate and eradicate the threat
-	Prioritize recovery according to endpoint criticality
-	Redeploy in a phased approach
-	Monitor restored endpoints for persistent malicious activity


## Associated Techniques

This countermeasure can be used to disrupt multiple adversary techniques.

- [T1001](https://attack.mitre.org/techniques/T1001/) - Data Obfuscation
- [T1003](https://attack.mitre.org/techniques/T1003/) - OS Credential Dumping
- [T1003.006](https://attack.mitre.org/techniques/T1003/006/) - OS Credential Dumping: DCSync
- [T1008](https://attack.mitre.org/techniques/T1008/) - Fallback Channels
- [T1011](https://attack.mitre.org/techniques/T1011/) - Exfiltration Over Other Network Medium
- [T1018](https://attack.mitre.org/techniques/T1018/) - Remote System Discovery
- [T1020](https://attack.mitre.org/techniques/T1020/) - Automated Exfiltration
- [T1021](https://attack.mitre.org/techniques/T1021/) - Remote Services
- [T1021.001](https://attack.mitre.org/techniques/T1021/001/) - Remote Services: Remote Desktop Protocol
- [T1021.004](https://attack.mitre.org/techniques/T1021/004/) - Remote Services: SSH
- [T1029](https://attack.mitre.org/techniques/T1029/) - Scheduled Transfer
- [T1030](https://attack.mitre.org/techniques/T1030/) - Data Transfer Size Limits
- [T1041](https://attack.mitre.org/techniques/T1041/) - Exfiltration Over C2 Channel
- [T1048](https://attack.mitre.org/techniques/T1048/) - Exfiltration Over Alternative Protocol
- [T1048.001](https://attack.mitre.org/techniques/T1048/001/) - Exfiltration Over Alternative Protocol: Exfiltration Over Symmetric Encrypted Non-C2 Protocol
- [T1048.002](https://attack.mitre.org/techniques/T1048/002/) - Exfiltration Over Alternative Protocol: Exfiltration Over Asymmetric Encrypted Non-C2 Protocol
- [T1048.003](https://attack.mitre.org/techniques/T1048/003/) - Exfiltration Over Alternative Protocol: Exfiltration Over Unencrypted Non-C2 Protocol
- [T1071](https://attack.mitre.org/techniques/T1071/) - Application Layer Protocol
- [T1071.001](https://attack.mitre.org/techniques/T1071/001/) - Application Layer Protocol: Web Protocols
- [T1071.002](https://attack.mitre.org/techniques/T1071/002/) - Application Layer Protocol: File Transfer Protocols
- [T1071.003](https://attack.mitre.org/techniques/T1071/003/) - Application Layer Protocol: Mail Protocols
- [T1071.004](https://attack.mitre.org/techniques/T1071/004/) - Application Layer Protocol: DNS
- [T1090](https://attack.mitre.org/techniques/T1090/) - Proxy
- [T1090.001](https://attack.mitre.org/techniques/T1090/001/) - Proxy: Internal Proxy
- [T1090.002](https://attack.mitre.org/techniques/T1090/002/) - Proxy: External Proxy
- [T1090.003](https://attack.mitre.org/techniques/T1090/003/) - Proxy: Multi-hop Proxy
- [T1090.004](https://attack.mitre.org/techniques/T1090/004/) - Proxy: Domain Fronting
- [T1095](https://attack.mitre.org/techniques/T1095/) - Non-Application Layer Protocol
- [T1098](https://attack.mitre.org/techniques/T1098/) - Account Manipulation
- [T1098.003](https://attack.mitre.org/techniques/T1098/003/) - Account Manipulation: Additional Cloud Roles
- [T1102](https://attack.mitre.org/techniques/T1102/) - Web Service
- [T1104](https://attack.mitre.org/techniques/T1104/) - Multi-Stage Channels
- [T1105](https://attack.mitre.org/techniques/T1105/) - Ingress Tool Transfer
- [T1110](https://attack.mitre.org/techniques/T1110/) - Brute Force
- [T1110.003](https://attack.mitre.org/techniques/T1110/003/) - Brute Force: Password Spraying
- [T1110.004](https://attack.mitre.org/techniques/T1110/004/) - Brute Force: Credential Stuffing
- [T1114](https://attack.mitre.org/techniques/T1114/) - Email Collection
- [T1132](https://attack.mitre.org/techniques/T1132/) - Data Encoding
- [T1133](https://attack.mitre.org/techniques/T1133/) - External Remote Services
- [T1185](https://attack.mitre.org/techniques/T1185/) - Browser Session Hijacking
- [T1197](https://attack.mitre.org/techniques/T1197/) - BITS Jobs
- [T1205](https://attack.mitre.org/techniques/T1205/) - Traffic Signaling
- [T1205.001](https://attack.mitre.org/techniques/T1205/001/) - Traffic Signaling: Port Knocking
- [T1210](https://attack.mitre.org/techniques/T1210/) - Exploitation of Remote Services
- [T1219](https://attack.mitre.org/techniques/T1219/) - Remote Access Software
- [T1498](https://attack.mitre.org/techniques/T1498/) - Network Denial of Service
- [T1498.001](https://attack.mitre.org/techniques/T1498/001/) - Network Denial of Service: Direct Network Flood
- [T1498.002](https://attack.mitre.org/techniques/T1498/002/) - Network Denial of Service: Reflection Amplification
- [T1499.002](https://attack.mitre.org/techniques/T1499/002/) - Endpoint Denial of Service: Service Exhaustion Flood
- [T1534](https://attack.mitre.org/techniques/T1534/) - Internal Spearphishing
- [T1542](https://attack.mitre.org/techniques/T1542/) - Pre-OS Boot
- [T1542.005](https://attack.mitre.org/techniques/T1542/005/) - Pre-OS Boot: TFTP Boot
- [T1546](https://attack.mitre.org/techniques/T1546/) - Event Triggered Execution
- [T1546.003](https://attack.mitre.org/techniques/T1546/003/) - Event Triggered Execution: Windows Management Instrumentation Event Subscription
- [T1546.008](https://attack.mitre.org/techniques/T1546/008/) - Event Triggered Execution: Accessibility Features
- [T1550](https://attack.mitre.org/techniques/T1550/) - Use Alternate Authentication Material
- [T1550.001](https://attack.mitre.org/techniques/T1550/001/) - Use Alternate Authentication Material: Application Access Token
- [T1550.004](https://attack.mitre.org/techniques/T1550/004/) - Use Alternate Authentication Material: Web Session Cookie
- [T1557](https://attack.mitre.org/techniques/T1557/) - Adversary-in-the-Middle
- [T1557.001](https://attack.mitre.org/techniques/T1557/001/) - Adversary-in-the-Middle: LLMNR/NBT-NS Poisoning and SMB Relay
- [T1557.003](https://attack.mitre.org/techniques/T1557/003/) - Adversary-in-the-Middle: DHCP Spoofing
- [T1558](https://attack.mitre.org/techniques/T1558/) - Steal or Forge Kerberos Tickets
- [T1558.003](https://attack.mitre.org/techniques/T1558/003/) - Steal or Forge Kerberos Tickets: Kerberoasting
- [T1563](https://attack.mitre.org/techniques/T1563/) - Remote Service Session Hijacking
- [T1565](https://attack.mitre.org/techniques/T1565/) - Data Manipulation
- [T1565.002](https://attack.mitre.org/techniques/T1565/002/) - Data Manipulation: Transmitted Data Manipulation
- [T1567](https://attack.mitre.org/techniques/T1567/) - Exfiltration Over Web Service
- [T1567.001](https://attack.mitre.org/techniques/T1567/001/) - Exfiltration Over Web Service: Exfiltration to Code Repository
- [T1567.002](https://attack.mitre.org/techniques/T1567/002/) - Exfiltration Over Web Service: Exfiltration to Cloud Storage
- [T1568](https://attack.mitre.org/techniques/T1568/) - Dynamic Resolution
- [T1570](https://attack.mitre.org/techniques/T1570/) - Lateral Tool Transfer
- [T1571](https://attack.mitre.org/techniques/T1571/) - Non-Standard Port
- [T1572](https://attack.mitre.org/techniques/T1572/) - Protocol Tunneling
- [T1573](https://attack.mitre.org/techniques/T1573/) - Encrypted Channel
- [T1573.001](https://attack.mitre.org/techniques/T1573/001/) - Encrypted Channel: Symmetric Cryptography
- [T1573.002](https://attack.mitre.org/techniques/T1573/002/) - Encrypted Channel: Asymmetric Cryptography
- [T1590.002](https://attack.mitre.org/techniques/T1590/002/) - Gather Victim Network Information: DNS



## Related Countermeasures

- CM0048 - Disable Hyper-V
- CM0022 - Quarantine Suspicious Files
- CM0023 - Identify and Terminate Suspicious Processes
- CM0059 - Configure Tactical Privileged Access Workstation
- CM0024 - Disconnect Internet-facing Boundary Controllers
- CM0027 - Rebuild Compromised Host
- CM0030 - Remove Known Malware
- CM0034 - Identify and Block Suspicious Hosts
- CM0137 - Remove Global Catalog from Domain Controller

## References

- IR in focus: Isolating & containing a confirmed threat | <https://redcanary.com/blog/incident-response/ir-containment-isolation/>
- Endpoint Isolation | <https://thewatchman.pro/endpoint-isolation/>
