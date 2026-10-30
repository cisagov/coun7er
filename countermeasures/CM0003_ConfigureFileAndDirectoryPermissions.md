# Configure File and Directory Permissions

* **ID:** CM0003
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Systems administration; file systems and access permissions

## Intended Outcome

Configuring file and directory permissions blocks adversary privilege escalation using lax permissions.

## Introduction

Critical system files and directories play an integral part in the
processes and functions of a system. The specific files and directories will vary by operating system. Examples include:

Windows: C:\Windows\System32\\*, C:\Windows\system.ini, C:\config.sy

Linux: /etc/\*, /sbin/\*, /bin/\*, /lib/system/\*, /boot/\*

Mac: ~/System/\*, /sbin/\*, /Library/\*, /bin/\*

## Preparation

1. Establish and review the baseline of file permissions.
2. Ensure access to an operating system-appropriate file permission viewer.
3. Identify which files you wish to verify the permissions of, which may involve finding/creating a list of critical system files and directories.

## Risks

- Corrupting or deleting critical system files can lead to system failures.
- Incorrect identification of a malicious permission allows the adversary to continue to operate, while incorrect identification of a legitimate permission can break legitimate functionality. A baseline of legitimate permissions for files can reduce the
likelihood of implementation errors.

## Guidance

The Reference section lists some of the most popular tools for file permission verification and configuration (grouped by operating system). Please consult the associated manuals for usage guidance. 

## Associated Techniques

- [T1036](https://attack.mitre.org/techniques/T1036) - Masquerading
- [T1036.003](https://attack.mitre.org/techniques/T1036/003) - Masquerading: Rename System Utilities
- [T1036.005](https://attack.mitre.org/techniques/T1036/005) - Masquerading: Match Legitimate Name or Location
- [T1037](https://attack.mitre.org/techniques/T1037) - Boot or Logon Initialization Scripts
- [T1037.002](https://attack.mitre.org/techniques/T1037/002) - Boot or Logon Initialization Scripts: Login Hook
- [T1037.003](https://attack.mitre.org/techniques/T1037/003) - Boot or Logon Initialization Scripts: Network Logon Script
- [T1037.004](https://attack.mitre.org/techniques/T1037/004) - Boot or Logon Initialization Scripts: RC Scripts
- [T1037.005](https://attack.mitre.org/techniques/T1037/005) - Boot or Logon Initialization Scripts: Startup Items
- [T1048](https://attack.mitre.org/techniques/T1048) - Exfiltration Over Alternative Protocol
- [T1053.006](https://attack.mitre.org/techniques/T1053/006) - Scheduled Task/Job: Systemd Timers
- [T1055.009](https://attack.mitre.org/techniques/T1055/009) - Process Injection: Proc Memory
- [T1070](https://attack.mitre.org/techniques/T1070) - Indicator Removal
- [T1070.001](https://attack.mitre.org/techniques/T1070/001) - Indicator Removal: Clear Windows Event Logs
- [T1070.002](https://attack.mitre.org/techniques/T1070/002) - Indicator Removal: Clear Linux or Mac System Logs
- [T1070.003](https://attack.mitre.org/techniques/T1070/003) - Indicator Removal: Clear Command History
- [T1070.008](https://attack.mitre.org/techniques/T1070/008) - Indicator Removal: Clear Mailbox Data
- [T1070.009](https://attack.mitre.org/techniques/T1070/009) - Indicator Removal: Clear Persistence
- [T1080](https://attack.mitre.org/techniques/T1080) - Taint Shared Content
- [T1098.004](https://attack.mitre.org/techniques/T1098/004) - Account Manipulation: SSH Authorized Keys
- [T1218.002](https://attack.mitre.org/techniques/T1218/002) - System Binary Proxy Execution: Control Panel
- [T1222](https://attack.mitre.org/techniques/T1222) - File and Directory Permissions Modification
- [T1222.001](https://attack.mitre.org/techniques/T1222/001) - File and Directory Permissions Modification: Windows File and Directory Permissions Modification
- [T1222.002](https://attack.mitre.org/techniques/T1222/002) - File and Directory Permissions Modification: Linux and Mac File and Directory Permissions Modification
- [T1489](https://attack.mitre.org/techniques/T1489) - Service Stop
- [T1530](https://attack.mitre.org/techniques/T1530) - Data from Cloud Storage
- [T1543](https://attack.mitre.org/techniques/T1543) - Create or Modify System Process
- [T1543.001](https://attack.mitre.org/techniques/T1543/001) - Create or Modify System Process: Launch Agent
- [T1543.002](https://attack.mitre.org/techniques/T1543/002) - Create or Modify System Process: Systemd Service
- [T1546.004](https://attack.mitre.org/techniques/T1546/004) - Event Triggered Execution: Unix Shell Configuration Modification
- [T1546.013](https://attack.mitre.org/techniques/T1546/013) - Event Triggered Execution: PowerShell Profile
- [T1547.003](https://attack.mitre.org/techniques/T1547/003) - Boot or Logon Autostart Execution: Time Providers
- [T1547.013](https://attack.mitre.org/techniques/T1547/013) - Boot or Logon Autostart Execution: XDG Autostart Entries
- [T1548](https://attack.mitre.org/techniques/T1548) - Abuse Elevation Control Mechanism
- [T1548.003](https://attack.mitre.org/techniques/T1548/003) - Abuse Elevation Control Mechanism: Sudo and Sudo Caching
- [T1552](https://attack.mitre.org/techniques/T1552) - Unsecured Credentials
- [T1552.001](https://attack.mitre.org/techniques/T1552/001) - Unsecured Credentials: Credentials In Files
- [T1552.004](https://attack.mitre.org/techniques/T1552/004) - Unsecured Credentials: Private Keys
- [T1553.003](https://attack.mitre.org/techniques/T1553/003) - Subvert Trust Controls: SIP and Trust Provider Hijacking
- [T1556](https://attack.mitre.org/techniques/T1556) - Modify Authentication Process
- [T1562](https://attack.mitre.org/techniques/T1562) - Impair Defenses
- [T1562.001](https://attack.mitre.org/techniques/T1562/001) - Impair Defenses: Disable or Modify Tools
- [T1562.002](https://attack.mitre.org/techniques/T1562/002) - Impair Defenses: Disable Windows Event Logging
- [T1562.004](https://attack.mitre.org/techniques/T1562/004) - Impair Defenses: Disable or Modify System Firewall
- [T1562.006](https://attack.mitre.org/techniques/T1562/006) - Impair Defenses: Indicator Blocking
- [T1563.001](https://attack.mitre.org/techniques/T1563/001) - Remote Service Session Hijacking: SSH Hijacking
- [T1564.004](https://attack.mitre.org/techniques/T1564/004) - Hide Artifacts: NTFS File Attributes
- [T1565](https://attack.mitre.org/techniques/T1565) - Data Manipulation
- [T1565.001](https://attack.mitre.org/techniques/T1565/001) - Data Manipulation: Stored Data Manipulation
- [T1565.003](https://attack.mitre.org/techniques/T1565/003) - Data Manipulation: Runtime Data Manipulation
- [T1569](https://attack.mitre.org/techniques/T1569) - System Services
- [T1569.002](https://attack.mitre.org/techniques/T1569/002) - System Services: Service Execution
- [T1574](https://attack.mitre.org/techniques/T1574) - Hijack Execution Flow
- [T1574.004](https://attack.mitre.org/techniques/T1574/004) - Hijack Execution Flow: Dylib Hijacking
- [T1574.007](https://attack.mitre.org/techniques/T1574/007) - Hijack Execution Flow: Path Interception by PATH Environment Variable
- [T1574.008](https://attack.mitre.org/techniques/T1574/008) - Hijack Execution Flow: Path Interception by Search Order Hijacking
- [T1574.009](https://attack.mitre.org/techniques/T1574/009) - Hijack Execution Flow: Path Interception by Unquoted Path

## Related Countermeasures

- CM0022 - Quarantine Suspicious Files
- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts
- CM0114 - Enable Microsoft Conditional Access Policy Templates
- CM0121 - Enable Managed Domain Authentication
- CM0133 - Reset Access Control Lists to Default Values

## References

- Center for Internet Security (CIS) Benchmarks | <https://learn.microsoft.com/en-us/compliance/regulatory/offering-CIS-Benchmark>
- Security baselines | <https://learn.microsoft.com/en-us/windows/security/threat-protection/windows-security-configuration-framework/windows-security-baselines>

### Windows 

- Find and open File Explorer | <https://support.microsoft.com/en-us/windows/find-and-open-file-explorer-ef370130-1cca-9dc5-e0df-2f7416fe1cb1>
- NTFSSecurity 4.2.4 | <https://www.powershellgallery.com/packages/NTFSSecurity/4.2.4>
- AccessChk v6.15 | <https://learn.microsoft.com/en-us/sysinternals/downloads/accesschk>
- AccessEnum v1.35 | <https://learn.microsoft.com/en-us/sysinternals/downloads/accessenum>
- icacls | <https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/icacls>
- 4670(S): Permissions on an object were changed | <https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4670>
- Sysinternals Utilities Index | <https://learn.microsoft.com/en-us/sysinternals/downloads/>
- Description of the Windows File Protection feature | <https://support.microsoft.com/en-us/topic/description-of-the-windows-file-protection-feature-db28f515-6512-63d1-6178-982ed2022ffb>
- What is the System32 Directory? (and Why You Shouldn't Delete It) | <https://www.howtogeek.com/346997/what-is-the-system32-directory-and-why-you-shouldnt-delete-it/>
- Use the System File Checker tool to repair missing or corrupted system files | <https://support.microsoft.com/en-us/topic/use-the-system-file-checker-tool-to-repair-missing-or-corrupted-system-files-79aa86cb-ca52-166a-92a3-966e85d4094e>
- File Integrity Monitoring in Microsoft Defender for Cloud | <https://learn.microsoft.com/en-us/azure/defender-for-cloud/file-integrity-monitoring-overview>

### Linux 

- ls(1) - Linux manual page | <https://www.man7.org/linux/man-pages/man1/ls.1.html>
- stat(2) - Linux manual page | <https://www.man7.org/linux/man-pages/man2/stat.2.html>
- GNOME/Nautilus | <https://github.com/GNOME/nautilus>
- Dolphin | <https://userbase.kde.org/Dolphin>

### MacOS 

- Organize your files in the Finder on Mac | <https://support.apple.com/guide/mac-help/organize-your-files-in-the-finder-mchlp2605/mac>
- How to gain access to the System folder on your Mac? | <https://macpaw.com/how-to/access-system-folder-mac>