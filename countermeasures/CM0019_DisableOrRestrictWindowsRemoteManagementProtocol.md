# Disable or Restrict Windows Remote Management (WinRM) Protocol

* **ID:** CM0019
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; WinRM and remote management

## Intended Outcome

Disabling or restricting the Windows Remote Management (WinRM) protocol
blocks or restricts adversary lateral movement using WS-Management
(WSMan) remoting in PowerShell.

## Introduction

WinRM may be required on select machines during incident response so that PowerShell remoting can be used to collect data from endpoints. This countermeasure discusses the disabling of WinRM, however, if certain systems require the protocol, establish a separate WinRM infrastructure complete with separate WinRM accounts and permissions.

## Preparation

- Establish and review the baseline behavior of WinRM in the environment.
- Determine necessity of permitting WinRM in the environment.
- Follow WinRM best practices on use of host firewalls to restrict WinRM access to/from specific devices.
- Enable firewall auditing to track changes made to the firewall exceptions policy and receive alerts if WinRM is enabled.

## Risks

- This countermeasure can break legitimate functionality.
- Disabling the WinRM protocol will limit usage of tools and services that require WS-Management/WINRM for PowerShell Remoting (including IR tools such as KANSA and PowerForensics).

## Guidance

### Disable WinRM

- WinRM can be disabled by executing PowerShell commands or by configuring the Group Policy (GP).
- Disable firewall exceptions for WS-Management communications. 

### Restrict WinRM

- If WinRM must be enabled, configure the GP via the GPMC. 

### Monitor WinRM

- Leverage Windows Event Logs to monitor for hosts that initialize (Event ID 6) and receive (Event ID 91) WinRM connections.     
- Use PowerShell module logging (Event ID 4103) to investigate what was executed after the WinRM connection was made. 

## Associated Techniques

- [T1021.006](https://attack.mitre.org/techniques/T1021/006) - Remote Services: Windows Remote Management
- [T1059.001](https://attack.mitre.org/techniques/T1059/001) - Command & Scripting Interpreter: PowerShell

## Related Countermeasures

- CM0001 - Disable Server Message Block (SMB) Protocol
- CM0012 - Disable or Restrict Distributed Component Object Model (DCOM) Protocol
- CM0013 - Block Windows Management Instrumentation (WMI) Service
- CM0017 - Enable Port Filtering on Host-based Firewalls
- CM0020 - Disable Virtual Network Computing (VNC) Desktop-sharing System
- CM0052 - Disable NetBIOS Service
- CM0025 - Disable Remote Desktop Protocol (RDP)
- CM0042 - Restrict Remote Desktop Protocol (RDP)
- CM0096 - Prevent and Detect Attempts to Copy NT Directory Services Database Information Table (NTDS.dit)
- CM0098 - Enable Extended Protection for Authentication (EPA)

## References

- Windows Remote Management | <https://learn.microsoft.com/en-us/windows/win32/winrm/portal>
- Security considerations for PowerShell Remoting using WinRM | <https://learn.microsoft.com/en-us/powershell/scripting/learn/remoting/winrmsecurity>
- Installation and configuration for Windows Remote Management | <https://learn.microsoft.com/en-us/windows/win32/winrm/installation-and-configuration-for-windows-remote-management>
- Disable-PSRemoting | <https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/disable-psremoting>
- Ransomware Protection and Containment Strategies: Practical Guidance for Endpoint Protection, Hardening, and Containment | <https://cloud.google.com/blog/topics/threat-intelligence/ransomware-protection-and-containment-strategies/>
- Investigating PowerShell Attacks | <https://www.mandiant.com/sites/default/files/2021-09/wp-lazanciyan-investigating-powershell-attacks.pdf>
- Disable PowerShell remoting: Disable-PSRemoting, WinRM, listener, firewall, LocalAccountTokenFilterPolicy | <https://4sysops.com/archives/disable-powershell-remoting-disable-psremoting-winrm-listener-firewall-and-localaccounttokenfilterpolicy/>
- Securing PowerShell in the Enterprise | <https://www.cyber.gov.au/resources-business-and-government/maintaining-devices-and-systems/system-hardening-and-administration/system-administration/securing-powershell-enterprise>
- 18.10.89.1.3 (L1) Ensure 'Disallow Digest authentication' is set to 'Enabled' | <https://www.tenable.com/audits/items/CIS_Microsoft_Windows_Server_2019_STIG_v2.0.0_L1_MS.audit:4bea4f57951af345a039dd713431eeef>
- 18.10.88.2.2 (L2) Ensure 'Allow remote server management through WinRM' is set to 'Disabled' | <https://www.tenable.com/audits/items/CIS_Microsoft_Windows_11_Stand-alone_v3.0.0_L2_BL.audit:d8dcd7935901b03d13a43119fa7af50f>
- 18.10.89.2.4 (L1) Ensure 'Disallow WinRM from storing RunAs credentials' is set to 'Enabled' | <https://www.tenable.com/audits/items/CIS_DC_SERVER_2012_Level_1_v3.0.0.audit:8b4ecce5f7751760d031ef407d50a6fd>
- 18.9.97.1.1 Ensure 'Allow Basic authentication' is set to 'Disabled' - Client | <https://www.tenable.com/audits/items/CIS_MS_Windows_7_v3.2.0_Level_1.audit:1e60e54b01648b82c665f8e42f7dd20d>
