# Block Applications in Writable Locations using AppLocker

* **ID:** CM0101
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; AppLocker rules and writable-path restrictions, Group Policy Management Console

## Intended Outcome

Blocking applications in writable locations using AppLocker blocks execution by malware in compromised environments. 

## Introduction

Adversaries deploy malware in furtherance of tactical objectives e.g. establish command-and-control or to achieve lateral movement, and persistence.  In the event of a large-scale, malware-enabled compromise, AppLocker can be employed to halt the pace and extent of adversarial advance on the network.  

AppLocker is an application control utility native to the Windows operating system (Windows 7 or more recent).  AppLocker enables responders to permit or deny the execution of specific applications in writable locations by users.  It can be configured to assess files based on the following attributes: digital signature, publisher, product, version, hash, and path.  

This countermeasure is specific to blocking the execution of malware to enable containment. To do so, AppLocker requires the location or hash of the malware.

AppLocker can block the following filetypes:

| File Type	| File Extension |
| :--------------| --------------:|
| Executables	| .exe, .dll, .ocx, .com	|
| Windows Installers | .mst, .msi, .msp |
| Script Files	| .bat, .ps1, .cmd, .js, .vbs |
| Packaged Application Installers	| .appx |


## Preparation

-	Enforce the principle of least privilege.  Ensure users are not members of the local administrator group.  AppLocker’s default rule set allows local administrators to run executables, scripts, and installer files.  If the adversary has elevated privileges, AppLocker’s default ruleset should be considered nullified until privileges have been revoked.     
-	If AppLocker is not already enabled, the Application Identify service (AppIDSvc) must be started to enable AppLocker.
-	Identify attributes of the malicious file e.g. path or file hash. 

## Risks

- Application control can be a necessary component of the incident response process.  It is not however, sufficient in and of itself, to stop the spread of malware, as it can be bypassed.  AppLocker policies will not impede an adversary with elevated privileges. 

## Guidance

AppLocker's deny list can be employed during incident response to contain the spread of malware.  

### Enable AppLocker and Apply Default Rules

-	Use the GPMC to navigate to `Security Settings> Application Control Policies> AppLocker> Executable Rules` in the Group Policy object.
-	Select `Executable Rules` and enable `Create Default Rules`.
-	Define `Deny` rules based on file attributes and characteristics associated with files determined to be malicious.  

#### Block by Location

AppLocker can be used to block execution by location.  If blocking files by location is an option, assess locations that non-admins have permission to write to, and execute from.  Take care not to block legitimate applications should they exist in these locations.  Malware is commonly deployed to the following locations:
-	`C:\Users\AppData\Local\Temp`
-	`C:\Users\AppData\Local`
-	`C:\Users\AppData`
-	`C:\Users\AppData\Roaming`
-	`C:\ProgramData`

#### Block by Filetype

AppLocker can be used to block filetypes from running in specific locations.  Again, if this approach is an option, take care not to block the execution of legitimate applications.  The following filetypes can be, but are not always, indicative of malware:

`.ade, .adp, .ani, .bas, .bat, .chm, .cmd, .com, .cpl, .crt, .hlp, .ht, .hta, .inf, .ins, .isp, .jar, .job, .js, .jse, .lnk, .mda, .mdb, .mde, .mdz, .msc, .msi, .msp, .mst, .ocx, .pcd, .ps1, .reg, .scr, .sct, .shs, .svg, .url, .vb, .vbe, .vbs, .wbk, .wsc, .ws, .wsf, .wsh, .exe, .pif, .pub`

### Monitor

Monitoring is an essential component of effective application control.  AppLocker allows rules to be configured to `Enforce` and/or `Audit`.  
`Audit` can be used during initial test and implementation to monitor effectiveness.  It should also be configured to enable continual monitoring. 

## Associated Techniques 

- [T1204.002](https://attack.mitre.org/techniques/T1204/002/) – User Execution: Malicious File
- [T1570](https://attack.mitre.org/techniques/T1570/) – Lateral Tool Transfer

## Related Countermeasures

- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0110 - Detect Anti-Malware Scan Interface (AMSI) Bypass Attempts
- CM0117 - Disable or Monitor Connection Manager Profile Installer (CMSTP.exe)
- CM0127 - Disable Microsoft XML Core Services (MSXSL.exe)
- CM0130 - Disable or Restrict Microsoft Build Engine (MSBuild.exe)
- CM0131 - Disable or Restrict Microsoft Management Console (MMC.exe)
- CM0132 - Disable or Restrict InstallUtil

## References

- Administer AppLocker | <https://learn.microsoft.com/en-us/windows/security/application-security/application-control/windows-defender-application-control/applocker/administer-applocker>
- Understanding AppLocker Default Rules | <https://learn.microsoft.com/en-us/windows/security/application-security/application-control/app-control-for-business/applocker/understanding-applocker-default-rules>
- How to Prevent Ransomware: 5 Practical Techniques and Countermeasures | <https://redcanary.com/blog/threat-detection/how-to-prevent-ransomware/>
