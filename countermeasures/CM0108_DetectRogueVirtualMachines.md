# Detect Rogue Virtual Machines

* **ID:** CM0108
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine, Eliminate
* **Status:** Active
* **Nice Work Role:** Defensive Cybersecurity (PD-WRL-001)
* **Skills & Knowledge:** Virtualization security monitoring; hypervisor logs and rogue virtual machine detection

## Intended Outcome

Detecting rogue virtual machines (VMs) identifies a common initial access and persistence technique employed in virtualized environments.

## Introduction

A rogue VM is a virtual machine that is created and deployed by an adversary to evade defenses, persist within and environment, and enable the adversary to expand access. Rogue VMs are created on the hypervisor to closely resemble legitimate virtual machines. Rogue VMs typically operate outside of the standard management process and do not adhere to security policies. As such, they cannot be identified in the vCenter inventory.  

## Preparation

- Patch vulnerabilities that may have afforded the adversary access to the environment. 
- Preserve artifacts to enable further analysis.

## Risks

- Identifying and removing a legitimate VM may be disruptive. Responders should take care not to eliminate a legitimate VM.

## Guidance

### Detect

- Continuously monitor SSH activity. Pay close attention to unexpected SSH enablement and SSH activation that do not coincide with administrative cycles. Anomalous SSH sessions can also indicate the presence of an adversary.

#### Detect Rogue VMs via ESXi Hypervisor Command-Line

- List all VMs: `vim-cmd vmsvc/getallvms`
- List all running VMs: `esxcli vm process list | grep Display`
- Compare the output of the two commands.  Discrepancies between the output of these two commands can indicate an issue. A VM that is not identifiable via the API-based VM check (vm-cmd vmsvc/getallvms) may indicate a potentially rogue VM.  

Alternatively, [Rogue VM Detection Script](https://github.com/center-for-threat-informed-defense/public-resources/tree/master/nerve-incident#rogue-vm-detection-script) and [VirtualGHOST](https://github.com/CrowdStrike/VirtualGHOST) are tools developed by MITRE's Center for Threat Informed Defense (CTID) and CrowdStrike to identify Rogue VMs.

#### Detect VMware Persistence Mechanisms

Continuously monitor for modification to the following:
-	`/etc/rc.local.d/local.sh`
-	`/bin/vmx -x/vmfs/volumes/<VOLUME>/<VM_NAME>/<VM_NAME>.vmx 2>/dev/null 0>/dev/null &`
-	Invocations of /bin/vmx binary in `/etc/rc.local.d/` or the `local.sh` startup script
-	`grep -r \/bin\/vmx /etc/rc.local.d/`
-	Cat /etc/rc.local.d/local.sh

### Eliminate

If a rogue virtual machine is detected, immediately preserve the VM, its memory, and all of its associated files for analysis. Isolate the VM and monitor network traffic. Destroying the VM should only be considered after an exhaustive analysis.  

- Create a snapshot to preserve the VM and memory: `vim-cmd vmsvc/snapshot.create <VMID> <SNAPSHOT_NAME> <SNAPSHOT DESCRIPTION> 1`.
- Destroy the rouge VM via the Hypervisor command-line: `vim-cmd vmsvc/destroy <VMID>`.
 

## Associated Techniques

-	[T1564.006](https://attack.mitre.org/techniques/T1564/006/) – Hide Artifacts: Run Virtual Instance

## Related Countermeasures

-   CM0067 - Install Critical Software Security Updates

## References

- MITRE: NERVE Incident | <https://github.com/center-for-threat-informed-defense/public-resources/tree/master/nerve-incident>
- Infiltrating Defenses: Abusing VMware in MITRE’s Cyber Intrusion | <https://medium.com/mitre-engenuity/infiltrating-defenses-abusing-vmware-in-mitres-cyber-intrusion-4ea647b83f5b>
- Memory Forensics for Virtualized Hosts | <https://blogs.vmware.com/security/2021/03/memory-forensics-for-virtualized-hosts.html>
- CTID Rogue VM Detection Script | <https://github.com/center-for-threat-informed-defense/public-resources/tree/master/nerve-incident#rogue-vm-detection-script>
- CrowdStrike VirtualGHOST | <https://github.com/CrowdStrike/VirtualGHOST>