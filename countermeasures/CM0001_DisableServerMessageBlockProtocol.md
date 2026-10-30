# Disable Server Message Block (SMB) Protocol

* **ID:** CM0001
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Systems administration; SMB and Windows file-sharing protocols

## Intended Outcome

Disabling the Server Message Block (SMB) protocol blocks adversary lateral movement to admin shares using SMB.

## Introduction

Adversaries can use the SMB protocol to remotely access administrative and hidden shares (those not mapped to a drive letter). 

## Preparation

- Estimate impact on business operations.
- Conduct risk assessment to understand whether potential disruption outweighs consequences of continued adversary access.
- Perform a backup of the registry before making any changes.

## Risks

This countermeasure can break legitimate functionality.

Disabling administrative and hidden shares on Domain Controllers increases this risk. For example, disabling the ADMIN$ share can:

- Inhibit the ability to use PsExec to remotely interface with endpoints
- Disrupt software updates and automated patching for third-party applications

Consider restricting access to shares by implementing an allowlist on the firewall as an alternative.

## Guidance

### Disable Administrative and Hidden Shares

To mitigate the adversary’s ability to write and execute from administrative and hidden shares and help contain an adversary by restricting their ability to remotely access systems, disable administrative or hidden shares from being accessible on endpoints.

### Disable Lanman Server via Services Console

File, print, and named-pipe sharing over the network can be disabled by opening the Services console (services.msc) on an endpoint, opening the Server service, and selecting disable from the Startup type drop down menu. The service can be likewise paused or stopped via the Server service configuration window.

### Disable Administrative and Hidden Shares via Registry

Admin and hidden shares can be disabled on endpoints by modifying the value in the registry by setting the AutoShareWks parameter to 0. For workstations, this will be at:

`HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters DWORD Name = “AutoShareWks” Value = “0”`

For servers, admin and hidden shares can be disabled at:

`HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters DWORD Name = “AutoShareServer” Value = “0”`

Note that this change will not disable the IPC\$ share. Although this share is not used for direct file access, it should still be disabled.

### Disable Administrative and Hidden Shares via GPO

Create a new Group Policy Object (GPO) named "Disable Administrative and Hidden Shares" or edit an existing GPO in the Group Policy Management Console. Then mark the following settings as disabled.

- Computer Configuration \> Policies \> Administrative Templates \> MSS(Legacy) \> MSS(AutoShareServer)
- Computer Configuration \> Policies \> Administrative Templates \> MSS(Legacy) \> MSS(AutoShareWks)

After establishing this GPO, link it to the appropriate Organizational Unit and apply the update to all relevant machines.

### Secure Accounts with Access to Hidden and Admin Shares

Accounts with access to hidden and admin shares should be locked down to prevent an adversary from leveraging the privileged accounts to conduct an attack. This includes considering the isolation of TCP 445 between machines or workstation subnets, where risks permit this.

- Administrative privileges should be regularly audited.
- Leverage Microsoft's Local Administrative Password Solution (LAPS).
- Accounts with access to admin or hidden shares should have logins configured to use multi-factor authentication (MFA).

## Associated Techniques

- [T1021.002](https://attack.mitre.org/techniques/T1021/002) - Remote Services: SMB/Windows Admin Shares

## Related Countermeasures

- CM0012 - Disable or Restrict Distributed Component Object Model (DCOM) Protocol
- CM0017 - Enable Port Filtering on Host-based Firewalls
- CM0019 - Disable or Restrict Windows Remote Management (WinRM) Protocol
- CM0020 - Disable Virtual Network Computing (VNC) Desktop-sharing System
- CM0052 - Disable NetBIOS Service
- CM0053 - Disable Link-Local Multicast Name Resolution (LLMNR) Service
- CM0025 - Disable Remote Desktop Protocol (RDP)
- CM0062 - Monitor or Block Server Message Block (SMB) Protocol
- CM0098 - Enable Extended Protection for Authentication (EPA)
- CM0100 - Restrict and Monitor Remote Procedure Calls (RPC)
- CM0141 - Audit and Restrict PsExec

## References

- Ransomware Protection and Containment Strategies: Practical Guidance for Endpoint Protection, Hardening, and Containment | <https://cloud.google.com/blog/topics/threat-intelligence/ransomware-protection-and-containment-strategies/>
- SMB/Windows Admin Shares | <https://redcanary.com/threat-detection-report/techniques/windows-admin-shares/>
- Remove administrative shares - Windows Server | <https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/remove-administrative-shares>
- Administrative shares are missing - Windows Server | <https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/problems-administrative-shares-missing>
