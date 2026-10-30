# Disable or Restrict Distributed Component Object Model (DCOM) Protocol

* **ID:** CM0012
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; DCOM configuration and remote services

## Intended Outcome

Disabling or restricting the Distributed Component Object Model (DCOM)
protocol blocks or restricts adversary lateral movement using malicious
COM objects.

## Introduction

DCOM is a technology that enables software component communication and interaction between software components directly over a network enabling distributed computing and inter-process communication. In many organizations, DCOM has been replaced by the .NET Framework, RESTful APIs, and Web Services. However, DCOM is still an essential technology for some organizations because it is required by some applications and legacy systems.

## Preparation

- Determine necessity of permitting DCOM in the environment.
- Determine whether blocking the protocol to specific systems, such as kiosks, fileservers, etc. is sufficient to harden security.

## Risks

- This countermeasure may block legitimate activity. This countermeasure should only be implemented by organizations for which DCOM is not an essential technology.
- Disabling DCOM may impact the functionality of applications and services that rely on it. Potential problems include:
    - Any COM objects that can be started remotely may not function correctly.
    - The local COM+ snap-in will not be able to connect to remote servers to enumerate their COM+ catalog.
    - Certificate auto-enrollment may not function correctly.
    - WMI queries against remote servers may not function correctly.
    - Disabling DCOM by modifying the registry may occur if the registry is modified incorrectly. To limit the damage caused by incorrectly modifying the registry, administrators should modify the registry using the GPO method and ensure that the registry is backed up prior to attempted modifications.

## Guidance

If reliant on DCOM, organizations should ensure all updates to the
protocol\'s implementation have been installed, including DCOM updates
released by Microsoft. 

Otherwise, if DCOM is not required, follow the guidelines below.

### Disable DCOM using GPO

DCOM can be disabled across multiple systems using a Group Policy Object
(GPO) created or modified within the Group Policy Management Console.
Specifically, system administrators can disable DCOM by selecting the
\"Enabled\" option for \"Default Properties\" located at Computer
Configuration \> Policies \> Administrative Templates \> System \>
Distributed COM

### Disable DCOM using Dcomcnfg.exe

DCOM can be disabled by running Dcomcnfg.exe and clearing the \"Enable
Distributed COM on this Computer\" checkbox within the Default
Properties tab of the window. Afterwards, administrators should apply
the changes and restart the operating system for the changes to take
effect. Dcomcnfg.exe can be further used to configure DCOM, including
access to highly sensitive
objects.

### Block DCOM with Windows Firewall

Under appropriate circumstances, security teams may want to consider
restricting DCOM on systems by blocking port 135 to reduce DCOM
functionality. DCOM uses port 135 for the initial session creation.
Blocking port 135 will inhibit remote procedure call (RPC) and remote
management of endpoints disrupting communication between workstations
and Domain Controllers and disrupting SMB file-sharing on Windows
servers.

### Harden Systems

Verify that other mitigations that prevent an adversary from enabling DCOM or potentially performing other malicious activities are implemented alongside any decision to disable or restrict DCOM. This may require disabling the remote registry and implementing GPOs which harden access privileges.

### Create DCOM Baseline

Security teams should create a baseline for DCOM usage in their networks
so anomalous DCOM activity can be detected.

## Associated Techniques

-   [T1021.003](https://attack.mitre.org/techniques/T1021/003) - Remote Services: Distributed Component Object Model

## Related Countermeasures

- CM0001 - Disable Server Message Block (SMB) Protocol
- CM0013 - Block Windows Management Instrumentation (WMI) Service
- CM0019 - Disable or Restrict Windows Remote Management (WinRM) Protocol
- CM0020 - Disable Virtual Network Computing (VNC) Desktop-sharing System
- CM0025 - Disable Remote Desktop Protocol (RDP)
- CM0042 - Restrict Remote Desktop Protocol (RDP)
- CM0100 - Restrict and Monitor Remote Procedure Calls (RPC)

## References

- How to disable DCOM support in Windows | <https://support.microsoft.com/en-us/topic/how-to-disable-dcom-support-in-windows-2bb8c280-9698-7f9c-bf67-2625a5873c7b>
- New lateral movement techniques abuse DCOM technology | <https://www.cybereason.com/blog/dcom-lateral-movement-techniques>
- KB5004442---Manage changes for Windows DCOM Server Security Feature Bypass (CVE-2021-26414) | <https://support.microsoft.com/en-us/topic/kb5004442-manage-changes-for-windows-dcom-server-security-feature-bypass-cve-2021-26414-f1400b52-c141-43d2-941e-37ed901c769c>
- Lateral Movement using DCOM Objects - How to do it the right way? | <https://www.scorpiones.io/articles/lateral-movement-using-dcom-objects>
