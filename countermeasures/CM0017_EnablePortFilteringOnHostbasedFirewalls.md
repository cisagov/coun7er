# Enable Port Filtering on Host-based Firewalls

* **ID:** CM0017
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Host firewall administration; ports and network filtering

## Intended Outcome

Enabling port filtering on host-based firewalls restricts adversary
lateral movement using subnet hosts.

## Introduction

Organizations may be able to provide sufficient security through a combination of endpoint detection and response (EDR) and network access control (NAC) solutions on endpoints. However, using host-based firewalls in combination with EDR and NAC provides layered defense.

## Preparation

- Organizations should manage their host-based firewalls with a centrally-managed service, such as Windows Group Policy Management Console (GPMC) or an alternate vendor-provided solution. 
- System administrators should consult internal policies and vendor instructions on how to appropriately configure their host-based firewalls and any other relevant NAC software.
- Host-based firewalls should be configured with a default deny rule, only allowing services and ports that are explicitly permitted.

## Risks

- This countermeasure can break legitimate functionality.
- Port filtering may block legitimate activity and break legitimate functionality.

## Guidance

### Windows

- To verify whether host-based firewalls are enabled by default on all hosts in a subnet, system administrators can launch GPMC and verify whether a Group Policy Object (GPO) exists that manages the firewall settings on all machines in the respective organizational unit.
- If host-based firewalls are not enabled by default on the affected subnet, organizations should identify the appropriate steps to isolate impacted systems from non-impacted systems and networks.

### Mac

- Each mobile device management (MDM) solution vendor may implement Mac firewall settings differently. Organizations that have introduced Macs into their environment should consult their vendor’s documentation.

## Associated Techniques

- [T1021](https://attack.mitre.org/techniques/T1021) - Remote Services
- [T1562.004](https://attack.mitre.org/techniques/T1562/004) - Impair Defenses: Disable or Modify System Firewall

## Related Countermeasures

- CM0001 - Disable Server Message Block (SMB) Protocol
- CM0019 - Disable or Restrict Windows Remote Management (WinRM) Protocol
- CM0020 - Disable Virtual Network Computing (VNC) Desktop-sharing System
- CM0025 - Disable Remote Desktop Protocol (RDP)
- CM0042 - Restrict Remote Desktop Protocol (RDP)
- CM0141 - Audit and Restrict PsExec
- CM0143 - Audit and Configure PI Web API

## References

- Open the Group Policy Management Console to Windows Firewall with Advanced Security | <https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/open-the-group-policy-management-console-to-windows-firewall-with-advanced-security>
- Configure rules with group policy | <https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/configure>
- A host-based firewall must be installed and enabled on the system. | <https://www.stigviewer.com/stig/microsoft_windows_server_2016/2021-03-05/finding/V-224846>
- Firewall MDM payload settings for Apple devices | <https://support.apple.com/guide/deployment/firewall-payload-settings-dep8d306275f/web>
