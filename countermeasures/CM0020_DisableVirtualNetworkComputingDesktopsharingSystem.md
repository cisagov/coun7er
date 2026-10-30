# Disable Virtual Network Computing (VNC) Desktop-sharing System

* **ID:** CM0020
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Systems administration; VNC and remote desktop services

## Intended Outcome

Disabling the Virtual Network Computing (VNC) desktop-sharing system
blocks adversary lateral movement using VNC.

## Introduction

Virtual Network Computing (VNC) is a graphical desktop-sharing system
that uses the Remote Frame Buffer protocol (RFB) to remotely control
another computer, relaying keyboard and mouse input to a server from a
client. Implementers of this countermeasure can consult available documentation
for different VNC implementations (LibVNC, TightVNC, UltraVNC, RealVNC).

## Preparation

- Identify organizational components that use the VNC system to perform remote troubleshooting, server management, and remote work and inform them that the system will be disabled.
- Ensure access to critical endpoints will remain accessible before disabling VNC, particularly if operational technology for critical infrastructure is in use.
- Coordinate with administrators to determine the extent VNC is used for administrative tasking.

## Risks

- This countermeasure can break legitimate functionality.
- Disabling VNC will limit remote access to endpoints (follow guidelines in Preparation section).
- This countermeasure may not stop all ongoing activity.
- Disabling VNC will not prevent remote access if other means of remote access, such as RDP, are still available and unsecure.

## Guidance

### Kill the VNC Service

- If VNC cannot be found but is being used by the adversary, hunt for suspicious processes that may be obfuscated VNC services. For guidance, see CM0023 - Identify and Terminate Suspicious Process.

### Block VNC traffic via Firewall

- By default, VNC uses TCP ports 5900 for the server, 5800 for browser access, and 5500 for a viewer in listening mode. Host and network inbound and outbound rules can be configured to deny VNC access.

### Disable VNC Service via GPO

- Create a new Group Policy Object (GPO) or edit an existing GPO in the Group Policy Management Console to disable the VNC service.
- After establishing this GPO, link it to the appropriate Organizational Unit (OU) and apply the update to all relevant computers.

### Block Execution of VNC.exe

- Third-party endpoint protection software can be used to define a rule to block the execution of vnc.exe.
- Windows Attack Surface Reduction (ASR) rules can also be used to block the execution of specific processes, including VNC.exe. For instructions on using ASR, see RM000X.

## Associated Techniques

- [T1021](https://attack.mitre.org/techniques/T1021) - Remote Services
- [T1021.005](https://attack.mitre.org/techniques/T1021/005) - Remote Services: VNC
- [T1071](https://attack.mitre.org/techniques/T1071) - Application Layer Protocol
- [T1078](https://attack.mitre.org/techniques/T1078) - Valid Accounts

## Related Countermeasures

- CM0001 - Disable Server Message Block (SMB) Protocol
- CM0048 - Disable Hyper-V
- CM0012 - Disable or Restrict Distributed Component Object Model (DCOM) Protocol
- CM0017 - Enable Port Filtering on Host-based Firewalls
- CM0019 - Disable or Restrict Windows Remote Management (WinRM) Protocol
- CM0052 - Disable NetBIOS Service
- CM0025 - Disable Remote Desktop Protocol (RDP)
- CM0062 - Monitor or Block Server Message Block (SMB) Protocol
- CM0032 - Revoke and Regenerate SSH Keys
- CM0042 - Restrict Remote Desktop Protocol (RDP)
- CM0098 - Enable Extended Protection for Authentication (EPA)

## References

- Setting up VNC Connect for Maximum Security | <https://help.realvnc.com/hc/en-us/articles/360002253278-Setting-up-VNC-Connect-for-Maximum-Security>
