#  Disable Hyper-V

* **ID:** CM0048
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; Hyper-V and virtualization services

## Intended Outcome 

Disabling the native Windows Hyper-V hypervisor blocks adversary privilege escalation, lateral movement, and potential C2 capability facilitated by hyperjacking and/or host escape techniques. 

## Introduction 

Hyper-V is a virtualization technology that allows users to create and run virtual machines (VMs) on Windows operating systems. Hyper-V (otherwise known as Viridian or Windows Server Virtualization) has personal and enterprise use cases across hardware virtualization. This countermeasure most closely remediates 'hyperjacking' attempts; hyperjacking involves an attacker installing malicious hypervisors or taking control of the hypervisor within a VM host.

Although there are procedures that reduce the risk of hyperjacking such as ensuring isolation of virtual machines behind firewalls and conducting patching of hypervisors, disabling the native Hyper-V hypervisor on Windows machines where it is unnecessary or underutilized most effectively prevents an adversaries' unauthorized control of virtual machines. 

## Preparation 

- Assess the enterprise network to determine system and service dependencies on Hyper-V. 
	- Type `msinfo32.exe` in Windows search box -> Select *System Information*
	- Locate entry `A hypervisor has been detected. Features required for Hyper-V will not be displayed.`
- Conduct backup of data on virtual machines managed by Hyper-V.
- Prior to disabling Hyper-V, ensure proper forensic information is collected on relevant virtual machines.
- Evaluate possible replacements to Hyper-V technology such as Citrix or Red Hat solutions.

## Risks 

- Disabling Hyper-V may disrupt services or the initialization of VMs altogether on host systems.
- Countermeasure may introduce compatibility issues with applications or services that rely on Hyper-V.
- Disabling Hyper-V may increase vulnerability of systems to downgrade attacks.

## Guidance 

### Disable Hyper-V via Control Panel

- Open the Windows **Control Panel** -> **Programs** -> **Programs and Features** -> *Turn Windows features on or off* 
	- Locate and expand the folder option titled *Hyper-V*.
	- Uncheck the *Hyper-V Hypervisor* check box.
	- Restart the computer to apply changes.

### Disable Hyper-V via PowerShell

- Open an administrator PowerShell terminal
	- Run the following command `Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`
	- Restart the computer to apply changes.

### Disable Hyper-V via BIOS/UEFI

- Restart the computer to enter BIOS/UEFI settings
- Locate the **Configuration** tab.
- Locate the Virtualization Technology option (**Intel Virtual Technology**) to set to **Disabled**.
- Save changes and exit the BIOS/UEFI settings.

### Verify Hyper-V is Disabled

- Ensure that Hyper-V is disabled by executing the following PowerShell command in an elevated prompt:
	-  `Get-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V`
	- The returned **State** parameter should contain a value of '**Disabled**.

## Associated Techniques 

- [T1611](https://attack.mitre.org/techniques/T1611/) - Escape to Host
- [T1610](https://attack.mitre.org/techniques/T1610/) - Deploy Container

## Related Countermeasures

- CM0020 - Disable Virtual Network Computing (VNC) Desktop-sharing System
- CM0065 - Isolate Endpoints from Network
- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts

## References 

- Introduction to Hyper-V on Windows 10 | <https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/about/>
- What is a Hyperjacking Attack and Are You at Risk? | <https://www.makeuseof.com/what-is-hyperjacking-attack/>
- Virtualization application don't work together with Hyper-V, Device Guard, and Credential Guard | <https://learn.microsoft.com/en-us/troubleshoot/windows-client/application-management/virtualization-apps-not-work-with-hyper-v>
- 5 Easy Ways to Disable Hyper-V in Windows 10 and 11 | <https://www.guidingtech.com/how-to-disable-hyper-v-windows/>
- Mitigate VMware Vulnerabilities | <https://www.cisa.gov/news-events/directives/ed-22-03-mitigate-vmware-vulnerabilities>
