# Identify and Monitor Remote Access Tools

* **ID:** CM0036
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active
* **Nice Work Role:** Defensive Cybersecurity (PD-WRL-001)
* **Skills & Knowledge:** Endpoint/security monitoring; remote access tools and threat detection

## Intended Outcome

Disabling or restricting remote access tools blocks or restricts
adversary persistence, lateral movement, and exfiltration using remote
access tools.

## Introduction

Some of the most common remote access tools include TeamViewer, AnyDesk, VNC Connect, Remote Desktop Protocol (RDP), Secure Shell (SSH), Virtual Private Networks (VPNs), and Windows Remote Desktop Services (RDS).

Security professionals need to be aware of all remote access tools enabled within their environment. In addition, security operations should consider assessing visibility into remote access tool usage and affirm strong remote access application monitoring practices if tools are used on the organization's network.

## Preparation

- Determine what is necessary for day-to-day operations prior to disabling native remote access applications.

## Risks

- This countermeasure may have impacts that disrupt operations.
- Limiting remote access tools can lead to reduced operational flexibility and convenience.

## Guidance

- Determine the presence of existing remote access tools (TeamViewer, AnyDesk, VNC Connect, Remote Desktop Protocol (RDP), SSH, VPNs, Windows Remote Desktop Services (RDS), etc.) and ensure remote access activity is being logged and monitored. 
- Identify open ports commonly used for remote access (20 (FTP), 21 (FTP), 22 (SSH), 23 (Telnet), 3389 (Microsoft Remote Display Protocol)).  Note, the absence of these ports does not negate the presence of remote access tools/protocols.
- Ensure the use of secure protocols such as Secure File Transfer Protocol (SFTP) and Secure Shell (SSH).
- Alert on persistent/prolonged remote connections and implement idle session timeout.

## Associated Techniques

- [T1018](https://attack.mitre.org/techniques/T1018) - Remote System Discovery
- [T1021](https://attack.mitre.org/techniques/T1021) - Remote Services
- [T1021.004](https://attack.mitre.org/techniques/T1021/004) - Remote Services: SSH
- [T1047](https://attack.mitre.org/techniques/T1047) - Windows Management Instrumentation
- [T1113](https://attack.mitre.org/techniques/T1113) - Screen Capture
- [T1218.003](https://attack.mitre.org/techniques/T1218/003) - System Binary Proxy Execution: CMSTP
- [T1219](https://attack.mitre.org/techniques/T1219) - Remote Access Software
- [T1563](https://attack.mitre.org/techniques/T1563) - Remote Service Session Hijacking

## Related Countermeasures

- CM0025 - Disable Remote Desktop Protocol (RDP)
- CM0032 - Revoke and Regenerate SSH Keys
- CM0042 - Restrict Remote Desktop Protocol (RDP)
- CM0109 - Audit and Restrict Cron Daemon
- CM0141 - Audit and Restrict PsExec

## References

- Technical Approaches to Uncovering and Remediating Malicious Activity | <https://www.cisa.gov/news-events/cybersecurity-advisories/aa20-245a>
- principle of least privilege (POLP) | <https://www.techtarget.com/searchsecurity/definition/principle-of-least-privilege-POLP>
- Guide to Enterprise Telework, Remote Access, and Bring Your Own Device (BYOD) Security | <https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-46r2.pdf>
