# Restrict Remote Desktop Protocol (RDP)

* **ID:** CM0042
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; RDP configuration and access controls

## Intended Outcome

Restricting the Remote Desktop Protocol (RDP) restricts adversary initial access and lateral movement using RDP.

## Introduction

RDP, also known as terminal services, allows users to remotely connect from one computer to another. This countermeasure restricts RDP. When RDP is needed, it should be made
accessible to users via a secure virtual private network (VPN) connection after authenticating via multi-factor authentication (MFA) or through a zero-trust remote access gateway.

## Preparation

- Estimate the impact of disabling RDP on business and sysadmin operations.
- Conduct a risk assessment with network owners and system administrators to understand whether potential short-term disruption outweighs the consequences of continued adversary access.

## Risks

- This countermeasure may have impacts that disrupt operations.
- Restricting RDP may disrupt legitimate operations that require the protocol.
- If lockouts are enforced, an attacker can create a denial of service condition.

## Guidance

### Rotate Passwords for RDP

- Limit RDP access by role-based access control and restrict to users that belong to a common security group.
- If user accounts with RDP access were compromised, rotate passwords associated with remote service logins.

### Disallow Administrative Accounts from Leveraging RDP on External-facing Systems

- Administrative accounts should be prohibited from interfacing with servers using RDP. 

### Enforce Account Lockouts

- Degrade the adversary’s ability to brute force an RDP login by configuring account lockouts. 
    - For Windows Authentication on remote server access, manually edit the remote access lockout settings in the remote access server's registry. For RADIUS, configure the registry on the Internet Authentication Server (IAS).
- Note that an attacker will still possess the ability to create a denial-of-service condition that intentionally locks out user accounts if lockouts are enforced. 
- Audit RDP Connection Logs. For example, the open source tool RdpMon can be used to show real-time and past RDP connections.

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

- CM0012 - Disable or Restrict Distributed Component Object Model (DCOM) Protocol
- CM0017 - Enable Port Filtering on Host-based Firewalls
- CM0019 - Disable or Restrict Windows Remote Management (WinRM) Protocol
- CM0020 - Disable Virtual Network Computing (VNC) Desktop-sharing System
- CM0025 - Disable Remote Desktop Protocol (RDP)
- CM0036 - Identify and Monitor Remote Access Tools

## References

- Ransomware Protection and Containment Strategies: Practical Guidance for Endpoint Protection, Hardening, and Containment | <https://cloud.google.com/blog/topics/threat-intelligence/ransomware-protection-and-containment-strategies/>
- Integrate your Remote Desktop Gateway infrastructure using the Network Policy Server (NPS) extension and Microsoft Entra ID | <https://learn.microsoft.com/en-us/entra/identity/authentication/howto-mfa-nps-extension-rdg>
- Configure remote access client account lockout | <https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/configure-remote-access-client-account-lockout>
- Remote Desktop Services must be configured with the client connection encryption set to the required level | <https://stigviewer.com/stig/windows_10/2020-06-15/finding/V-63741>
- Establishing a Baseline for Remote Desktop Protocol | <https://cloud.google.com/blog/topics/threat-intelligence/establishing-baseline-remote-desktop-protocol/>
- Deny log on through Remote Desktop Services | <https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/deny-log-on-through-remote-desktop-services>
- RdpMon - Server-side RDP monitoring tool | <https://github.com/cameyo/rdpmon>
