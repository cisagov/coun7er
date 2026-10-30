# Disable Remote Desktop Protocol (RDP)

* **ID:** CM0025
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; RDP and remote access controls

## Intended Outcome

Disabling the Remote Desktop Protocol (RDP) blocks adversary initial
access and lateral movement using RDP.

## Introduction

RDP, also known as terminal services, allows users to remotely connect
from one computer to another. This countermeasure disables RDP, but if
RDP is needed, it should be made accessible to users via a secure
virtual private network (VPN) connection after authenticating via
multi-factor authentication (MFA) or through a zero-trust remote access
gateway. Furthermore, if RDP access cannot be disabled, security teams should 
rotate RDP passwords and eliminate systems running RDP where possible. 

## Preparation

- Assess the impact to operations if RDP is disabled or restricted.
- Conduct risk assessment to understand whether potential disruption outweighs consequences of continued adversary access.

## Risks

- This countermeasure can break legitimate functionality.
- Disabling RDP can disrupt legitimate business functions.

## Guidance

- By default, RDP communicates over port 3389. System administrators should consider blocking RDP for all users that don't belong to specific approved security groups.
- RDP can be disabled via GPO or on specific hosts via PowerShell and the Windows Firewall.  

## Associated Techniques

- [T1021.001](https://attack.mitre.org/techniques/T1021/001) - Remote Services: Remote Desktop Protocol
- [T1049](https://attack.mitre.org/techniques/T1049) - System Network Connections Discovery
- [T1071](https://attack.mitre.org/techniques/T1071) - Application Layer Protocol
- [T1078](https://attack.mitre.org/techniques/T1078) - Valid Accounts
- [T1219](https://attack.mitre.org/techniques/T1219) - Remote Access Software
- [T1563.002](https://attack.mitre.org/techniques/T1563/002) - Remote Service Session Hijacking: RDP Hijacking
- [T1570](https://attack.mitre.org/techniques/T1570) - Lateral Tool Transfer
- [T1572](https://attack.mitre.org/techniques/T1572) - Protocol Tunneling

## Related Countermeasures

- CM0001 - Disable Server Message Block (SMB) Protocol
- CM0012 - Disable or Restrict Distributed Component Object Model (DCOM) Protocol
- CM0017 - Enable Port Filtering on Host-based Firewalls
- CM0019 - Disable or Restrict Windows Remote Management (WinRM) Protocol
- CM0020 - Disable Virtual Network Computing (VNC) Desktop-sharing System
- CM0036 - Identify and Monitor Remote Access Tools
- CM0042 - Restrict Remote Desktop Protocol (RDP)

## References

- Ransomware Protection and Containment Strategies: Practical Guidance for Endpoint Protection, Hardening, and Containment | <https://cloud.google.com/blog/topics/threat-intelligence/ransomware-protection-and-containment-strategies/>
- Integrate your Remote Desktop Gateway infrastructure using the Network Policy Server (NPS) extension and Microsoft Entra ID | <https://learn.microsoft.com/en-us/entra/identity/authentication/howto-mfa-nps-extension-rdg>
- Configure remote access client account lockout | <https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/configure-remote-access-client-account-lockout>
- Remote Desktop Services must be configured with the client connection encryption set to the required level. | <https://stigviewer.com/stig/windows_10/2020-06-15/finding/V-63741>
- Establishing a Baseline for Remote Desktop Protocol | <https://cloud.google.com/blog/topics/threat-intelligence/establishing-baseline-remote-desktop-protocol/>
- Deny log on through Remote Desktop Services | <https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/deny-log-on-through-remote-desktop-services>
