# Eliminate Web Shells

* **ID:** CM0106
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Eliminate
* **Status:** Active
* **Nice Work Role:** Incident Response (PD-WRL-003)
* **Skills & Knowledge:** Web server incident response; web shell identification and removal, artifact analysis, digital forensics

## Intended Outcome

Eliminating web shells removes malicious code from vulnerable web servers that enable initial access and persistence.

## Introduction

A web shell is malicious code that is written to a vulnerable web server. 
Adversaries typically exploit server misconfigurations or software vulnerabilities to deploy web shells. Web shells provide adversaries with a foothold that enables them to execute commands, exfiltrate data, and upload additional files to expand access beyond the initial point of access.  

## Preparation

-	Verify that plans, procedures, and authorities are in place to enable rapid containment and eradication of adversary from the compromised server. 
-	Take steps to prepare for the operational interruptions incurred by taking a web server offline.   

## Risks

-	Eliminating web shells may require down-time and thus disrupt the availability of the compromised web server.  
-	Web shells are often used to achieve initial access and/or persistence. Additional tools, e.g. malware, are often deployed to enable the adversary to further compromise the environment. As such, additional countermeasures may be required to fully contain and eradicate the adversary from the compromised environment.    


## Guidance

### Detecting Web Shells

The following detection logic should be considered components of a broader defense-in-depth strategy, not stand-alone solutions. 

- Compare files on web server with a known-good version. [Windiff](https://learn.microsoft.com/en-us/troubleshoot/windows-client/shell-experience/how-to-use-windiff-utility) is a tool developed by Microsoft for file and directory comparison. Another option to consider is [dirChecker](https://github.com/nsacyber/Mitigating-Web-Shells/blob/master/dirChecker.ps1) by NSA Cyber.  
- Detect anomalous user agents, referrer headers, and IP Addresses in web server logs. This detection logic is likely to result in a high rate of false positives and should be continually monitored and refined.     
- Detect host-based artifacts for common web shells. Consider using a security scanning tool like [YARA](https://github.com/virustotal/yara/) and the [core web shell detection signatures](https://github.com/nsacyber/Mitigating-Web-Shells/blob/master/core.webshell_detection.yara)
- Detect network-based artifacts for common web shells.  
- Detect unexpected network flows.

### Eliminating Web Shells

-	Isolate the compromised web server to reduce the likelihood of lateral movement.
-	Preserve artifacts to enable further analysis. 
    - Disk and memory artifacts.
    - Logs (operating system, web application, access, error, web application firewall, reverse proxy, content delivery network, DMZ firewall, and database logs).
-	Delete the web shell.
-	Change passwords for the web administrator, database user, hosting account, and remote access (FTP, SSH, etc.)
-	Investigate the underlying vulnerability/cause of the breach.
-	Restore from clean backup.
-	Having identified the underlying vulnerability / misconfiguration that led to the breach remediate by patching and hardening the web application and/or web server. 

## Associated Techniques

-	[T1021](https://attack.mitre.org/techniques/T1021/) – Remote Services
-	[T1190](https://attack.mitre.org/techniques/T1190/) – Exploit Public-Facing Application
-	[T1505.003](https://attack.mitre.org/techniques/T1505/003/) – Server Software Component: Web Shell

## Related Countermeasures

- CM0024 - Disconnect Internet-facing Boundary Controllers
- CM0067 - Install Critical Software Security Updates
- CM0030 - Remove Known Malware
- CM0093 - Prevent and Detect Network Traffic to/from The Onion Router (Tor) Exit Nodes


## References

- Mitigating Web Shells | <https://github.com/nsacyber/Mitigating-Web-Shells>
- Threat Actors Exploit Multiple Vulnerabilities in Ivanti Connect Secure and Policy Secure Gateways | <https://www.cisa.gov/sites/default/files/2024-02/AA24-060B-Threat-Actors-Exploit-Multiple-Vulnerabilities-in-Ivanti-Connect-Secure-and-Policy-Secure-Gateways_0.pdf>
- Web Shells: Types, Mitigation & Removal | <https://blog.sucuri.net/2024/04/web-shells.html>
- Ghost in the Shell: Investigating Web Shell Attacks | <https://www.microsoft.com/en-us/security/blog/2020/02/04/ghost-in-the-shell-investigating-web-shell-attacks/>
- How to use Windiff.exe | <https://learn.microsoft.com/en-us/troubleshoot/windows-client/shell-experience/how-to-use-windiff-utility>
- dirChecker.ps1 | <https://github.com/nsacyber/Mitigating-Web-Shells/blob/master/dirChecker.ps1>
- YARA | <https://github.com/virustotal/yara/>
- YARA rules for detecting common web shells | <https://github.com/nsacyber/Mitigating-Web-Shells/blob/master/core.webshell_detection.yara>
