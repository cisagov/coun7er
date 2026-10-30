# Restore SYSVOL Content in AD Domains

* **ID:** CM0107
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Infrastructure Support (PD-WRL-004)
* **Skills & Knowledge:** Active Directory administration; SYSVOL restoration and Group Policy recovery

## Intended Outcome

Restoring SYSVOL content terminates adversary defense evasion, persistence, privilege escalation, lateral movement, and impact via malicious GPOs and malware staged in the SYSVOL directory.

## Introduction

Active directory environments are a prime target for organized threat groups. When executing containment efforts that aim to rebuild and recover on-premise Active Directory environments, a critical step is to perform a clean restoration of System Volume (SYSVOL) and/or Netlogon shares. SYSVOL is a shared folder that exists on the hard disk of a each domain controller within an Active Directory forest, containing functional components such as Group Policy Objects (GPOs) and logon/logoff scripts. For this reason, restoring SYSVOL content in AD environments is necessary for a victim to regain authentication capabilities linked to SYSVOL, as well as restored functionality and synchronization of a corrupted File Replication Service (FRS). In more modern implementations of active directory forests, FRS is expected to be replaced by the Distributed File System Replication (DFSR) service. DFSR provides more efficient and reliable file replication that deprecates FRS; organizations should migrate from FRS to DFSR to ensure better performance and support. Neglecting to restore SYSVOL after compromise invites an attacker to continuously harvest credentials and obscure malicious scripts that enable their persistence within a network.

## Preparation

- Determine whether SYSVOL requires an authoritative recovery or a non-authoritative recovery. If the AD forest is compromised, proceed with a full rebuild via authoritative restore.
- Identify content with the SYSVOL and Netlogon shared folders of each domain controller. Note any incongruencies across the AD environment.
- Examine and document the status of the FRS/DFSR service within the AD environment, noting inconsistencies and signs of corruption.
- If present, identify existing SYSVOL backups that can be used in authoritative recovery of SYSVOL.
- Conduct system backups before making changes to the SYSVOL tree.

## Risks

- Interacting with a domain's SYSVOL tree and its contents will cause domain controllers to stop service of authentication requests until the SYSVOL folder is shared again.
- Incorrect registry modification performed by following the included guidance may cause malfunction of system startup functions.
- Improper application of this countermeasure can disrupt the synchronization of files/content within the SYSVOL and Netlogon folders.

## Guidance

### Rebuild SYSVOL Domain Replica Set Across Domain Controllers

1. Stop the FRS/DFSR for all domain controllers in the domain. Set the service startup type value for the FRS/DFSR to **Disabled.**
2. Move all files and folders that are required to reside in the SYSVOL tree to a temporary folder on a staged domain controller. Place the temporary folder on the same partition that the SYSVOL tree is located. This domain controller will serve as the *reference domain controller*, containing the authoritative copy of the SYSVOL tree that will be replicated across all other domain controllers.
	- Configure the *reference domain controller* to be authoritative. 
3. Verify that all domain controllers share the following folders and reparse points:
	- `SYSVOL`
	- `SYSVOL\domain`
	- `\SYSVOL\staging\domain`
	- `SYSVOL\staging areas`
	- `SYSVOL\domain\Policies`
	- `\SYSVOL\domain\scripts`
	- `\SYSVOL\SYSVOL`
	- `SYSVOL\SYSVOL\DNS Domain Name` (linked to `\SYSVOL\domain`)
	- `\SYSVOL\staging areas\DNS Domain Name` (linked to `\SYSVOL\staging\domain`)
- Recreate the aforementioned folders/reparse points if they are missing. NOTE: avoid using Windows Explorer to move or copy contents of the SYSVOL tree to prevent breaking reparse points.
4. Start File Replication Service (FRS) on each domain controller to link reparse points:
	- Go to Start -> Run -> type `cmd` -> OK
	- Type `net start ntfrs` to start FRS
	- Type `ntfrsutl ds |findstr /i "root stage"` -> press Enter to verify service status
	- Type `Linkd %systemroot%\SYSVOL\SYSVOL\DNS Domain name` -> press Enter to link folder to domain junction point
	- Type `linkd "%systemroot%\SYSVOL\staging areas\DNS Domain Name"` -> press Enter to link folder to domain junction point
5. Verify that there is enough space for the staging across all domain controllers. Staging folder size may need to be adjusted.
6. Build and validate policies and scripts in a temporary folder on the *reference domain controller*. Map GPOs to SYSVOL folders.
7. Prepare the *reference domain controller* by moving validated policies and scripts from the temporary folder to `C:\WINNT\SYSVOL\domain`.
8. For all domain controllers other than the *reference domain controller*:
	- set `BurFlags` to `D2` in `regedit` under `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative Replica Sets\GUID`
9. Rebuild the SYSVOL tree in a staggered manner in order to avoid overloading a single domain controller. Ensure that the `Replica Set Parent` registry entry specifies a source domain controller.
10. Clean up *non-reference domain controllers*, deleting files and folders under `C:\WINNT\SYSVOL\domain` and `C:\WINNT\SYSVOL\staging\domain`. Ensure the these two folders are not deleted themselves.
11. Restart FRS on all domain controllers EXCEPT the *reference domain controller*, verifying that SYSVOL and NETLOGON are shared. Ensure that the FRS service startup type is set to `Automatic`.
	NOTE: A step-by-step version of this guidance that includes detailed explanation of commands and configurations can be obtained from the following Microsoft documentation:
	  https://learn.microsoft.com/en-us/troubleshoot/windows-server/group-policy/rebuild-sysvol-tree-and-content-in-a-domain#detailed-list-of-the-steps

### Perform FRS to DFSR SYSVOL Migration

- FRS is a legacy SYSVOL replication service that spans Windows Server operating systems from 2000-2008. DFSR has become the default replication engine for systems running Windows 2008 and newer. Due to the increased efficiency, performance, and security of DFSR, it is recommended to perform the migration procedures on Windows Server systems that rely on the deprecated FRS service.
    - In order to proceed with an FRS to DFSR SYSVOL migration, one must verify that the system uses FRS using the `dfsrmig /getglobalstate` command:
	    1. Log in to the domain controller as Domain admin or Enterprise admin
	    2. Launch the PowerShell console and type `dfsrmig /getglobalstate`. Executing this command produces expected console output "DFSR migration has not yet initialized..."
    - The migration process occurs in four phases that assigned a unique state. The four states are as follows:
	    - State 0 - Start
	    - State 1 - Prepared
	    - State 2 - Redirected
	    - Stage 3 - Eliminated
1. Ensure a current copy of the SYSVOL folder is created before beginning the migration process. The migration process will automatically start at State 0 where FRS will replicate the SYSVOL folder amongst the domain controllers. 
2. Log in to the domain controller as Domain/Enterprise admin to initiate *State 1 - Prepared*. 
	1. Launch the PowerShell console and execute the `dfsrmig /setglobalstate 1` command. With this completed, the DFSR state will be updated to 'Prepared.' 
	2. Confirm all domain controllers have reached the 'prepared' state by executing command `dfsrmig /getmigrationstate`.
3. Log in to the domain controller as Domain/Enterprise admin to initiate *State 2 - Redirected*.
	1. Launch the PowerShell console and execute the `dfsrmig /setglobalstate2` command. With this completed, the DFSR state will be updated to 'Redirected.'
	2. Confirm all domain controllers have reached the 'redirected' state by executing command `dfsrmig /getmigrationstate`.
4. Log in to the domain controller as Domain/Enterprise admin to initiate *State 3 - Eliminated*.
	1. Launch the PowerShell console and execute the `dfsrmig /setglobalstate3` command. With this completed, the DFSR state will be updated to 'Eliminated'.
	2. Confirm all domain controllers have reached the 'Eliminated' state by executing command `dfsrmig /getmigrationstate`.
5. Confirm the presence of the SYSVOL share by executing the `net share` command. The console output is expected to highlight the `\SYSVOL_DFSR\` share.
6. Ensure the FRS service (servicename *NtFrs*) is stopped and disabled across each domain controller.
	

### Perform an Authoritative Synchronization of DFSR-replicated SYSVOL

- In active directory environments that contain the modern DFSR file replication service, synchronization of the SYSVOL folder must be performed:
 1. Open Active Directory Users and Computers
 2. Select **View > Users, Contacts, Groups, and Computers as containers > Advanced Features**
 3. In the folder tree-view, select **Domain Controllers**, the name of the domain controller that has been restored, **DFSR-LocalSettings,** and then **Domain System Volume.**
 4. In the Details pane, right-click **SYSVOL Subscription**, select **Properties**, and select **Attribute Editor**.
 5. Select **msDFSR-Options**, select **Edit**, type **1**, and select **OK**.
 6. Select **OK** to close the Attribute Editor.

- In order to verify if the authoritative restore was successful, use PowerShell to execute the `Restart-Service DFSR -PassThru` command.
- Verify the presence of Event ID 4602 within Windows Event Logs:
	  `Get-WinEvent -LogName 'DFS Replication' | Where-Object ID -EQ 4602 | Format-Table -AutoSize -Wrap`

## Associated Techniques

- [T1484.001](https://attack.mitre.org/techniques/T1484/001/)- Domain or Tenant Policy Modification: Group Policy Modification
- [T1552.006](https://attack.mitre.org/techniques/T1552/006/)- Unsecured Credentials: Group Policy Preferences
- [T1080](https://attack.mitre.org/techniques/T1080/) - Taint Shared Content

## Related Countermeasures

- CM0018 - Enable Active Directory (AD) Protected Users Security Group
- CM0069 - Reset Directory Services Restore Mode (DSRM) Account Passwords on Domain Controllers
- CM0111 - Enable Multiple Administrative Approval (MAA) for Access Policies
- CM0122 - Restrict SYSVOL Credential Theft
- CM0139 - Rebuild Active Directory (AD) Connect Server
- CM0144 - Audit SYSVOL for Vulnerable Group Policy Preferences 
- CM0148 - Disable Cisco Open NX-OS Guest Shell
- CM0149 - Temporarily Disable Cisco Open NX-OS Guest Shell

## References

- How to Rebuild the SYSVOL Tree and its Content in a Domain | <https://learn.microsoft.com/en-us/troubleshoot/windows-server/group-policy/rebuild-sysvol-tree-and-content-in-a-domain>

- Securing SYSVOL: Threats, Protection, and Recovery | <https://www.cayosoft.com/sysvol/>

- A Queen's Ransom: Varonis Uncovers Fast-Spreading "SaveTheQueen" Ransomware | <https://www.varonis.com/blog/save-the-queen-ransomware>

- Active Directory Forest Recovery - Perform an authoritative synchronization of DFSR-replicated SYSVOL | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/forest-recovery-guide/ad-forest-recovery-authoritative-recovery-sysvol>

- The Procedure for FRS to DFSR SYSVOL Migration | <https://noynim.com/frs-to-dfsr-sysvol-migration-step-by-step/>


