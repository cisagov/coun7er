# Block High-risk File Types at Web Gateway

* **ID:** CM0047
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Infrastructure Support (PD-WRL-004)
* **Skills & Knowledge:** Web/email gateway administration; file-type filtering and content controls

## Intended Outcome

Blocking high-risk file types at the web gateway restricts adversary initial access and execution. 

## Introduction

A web gateway acts as an intermediary, situated between end-users and the internet.  Web gateways can be hosted on-premises or via the cloud.  The web gateway’s purpose is to inspect, analyze, and filter malicious inbound/outbound web traffic.  Web gateways enforce acceptable use and security policies.   

## Preparation

- Research the web gateway’s capabilities.
- Review policies currently applied to the web gateway.
- Identify file types necessary to operations.
- Identify file types that if blocked, could result in operational disruption.

## Risks

Blocking a file type that is necessary to operations can result in an operational disruption.  To mitigate the risk of disruption, ensure that blocked file types are not required.  Continually monitor the web gateway and adjust policies accordingly. 

## Guidance

This countermeasure assumes the organization is using a web gateway.  Please refer to vendor documentation for specific guidance.  

1.	Login to the web gateway management portal
2.	Locate the existing rule you want to edit or create a new rule
3.	Edit or populate the rule with content
4.	Save, add, and activate the rule
5.	Monitor the web gateway to ensure the newly created rule is effective

Though not exhaustive, the following file types could introduce risk.  Consider blocking executable file types.  Other file types can be blocked though operational necessity should be considered to mitigate the likelihood of disruption.   

| File Types         | File Extensions                                |
|:-------------------|:--------------------                           |
|Executable          |exe, scr, vbs, js, xml, docm, xps               |
|Image               |iso, img                                        |
|Archive/Compressed  |arj, lzh, r01, r14, r18, r25, tar, ace, zip, jar|
|Documents           |doc, rtf, xls, pdf, pub                         |

## Associated Techniques

- [T1189](https://attack.mitre.org/techniques/T1189/) – Drive-by Compromise
- [T1204.001](https://attack.mitre.org/techniques/T1204/001/) – User Execution: Malicious Link
- [T1204.002](https://attack.mitre.org/techniques/T1204/002) – User Execution: Malicious File 


## Related Countermeasures

- CM0002 - Enable Email Attachment Filtering and Message Authentication
- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0056 - Disable Visual Basic for Applications (VBA) Macros
- CM0116 - Restrict Visual Basic for Applications (VBA) Macros
- CM0143 - Audit and Configure PI Web API

## References

- File Types to Block | <https://docs.umbrella.com/umbrella-user-guide/docs/file-types>
- Internet Gateway Best Practice Security Policy | <https://docs.paloaltonetworks.com/best-practices/internet-gateway-best-practices/best-practice-internet-gateway-security-policy/transition-safely-to-best-practice-security-profiles/transition-file-blocking-profiles-safely-to-best-practices>
