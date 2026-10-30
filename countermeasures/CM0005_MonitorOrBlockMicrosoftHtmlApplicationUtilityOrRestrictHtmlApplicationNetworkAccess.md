# Monitor or Block Microsoft HTML Application (MSHTA) Utility or Restrict HTML Application (HTA) Network Access

* **ID:** CM0005
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine, Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; application control and network access restrictions

## Intended Outcome

Monitoring or blocking the Microsoft HTML Application (MSHTA) utility detects or blocks adversary execution of malicious HTML Application (HTA) programs. Restricting HTA network access blocks adversary defense evasion using proxy execution of malicious HTA programs.

## Introduction

Mshta.exe is a Windows utility that provides a host for HTA files to run in. Although it has legitimate uses, attackers can use mshta.exe to run malicious JavaScript or VBScript commands.

## Preparation

- Assess the impact to operations if mshta.exe is disabled or restricted.

## Risks

- This countermeasure can break legitimate functionality.
- Disabling mshta.exe may disable legitimate applications that rely on HTML Applications (HTA). Identify any legitimate custom-built applications that might be using HTA.

## Guidance

### Monitor

Configure the host-based agent to monitor for
- Process execution
- Command-line parameter inspection, and/or 
- Process lineage monitoring

To gain insight into attempts at binary masquerading, consider collecting process metadata. Adversaries have been observed renaming binaries to evade brittle detections.
- Monitor for network calls from the MSHTA binaries
- Monitor DLL's (mshta.dll, jscript9.dll)

### Block

#### AppLocker

- Utilize Windows AppLocker to assign a rule to prevent execution of mshta.exe and mshta.dll.

#### Disable Network Access

Firewall rules can be used to disable network access by HTA files. HTA files will still be able to execute, however, threat actors won’t be able to use HTA for attacks such as remote file inclusion, remote code execution, or other attacks that require internet access.

## Associated Techniques

-   [T1218.005](https://attack.mitre.org/techniques/T1218/005) - System binary Proxy Execution: Mshta

## Related Countermeasures

- CM0006 - Disable and Monitor Regasm.exe and Regsvcs.exe Utilities
- CM0007 - Disable and Monitor Verclsid.exe System Binary
- CM0008 - Block or Monitor Mavinject.exe Utility
- RM0027 - Prevent Execution of MMC.exe (*future*)
- RM0028 - Prevent Execution of msbuild.exe (*future*)
- CM0045 - Disable or Monitor Execution of WMIC.exe
- RM0030 - Prevent Execution of Unnecessary Scripts (*future*)
- RM0031 - Prevent Execution of InstallUtil.exe (*future*)
- RM0032 - Prevent Execution of Executables Masquerading as Other Files (*future*)
- CM0046 - Block Execution of Compiled HTML (CHM) Files
- CM0056 - Disable Visual Basic for Applications (VBA) Macros
- CM0091 - Prevent and Detect System Binary Proxy Execution
- CM0117 - Disable or Monitor Connection Manager Profile Installer (CMSTP.exe)
- CM0127 - Disable Microsoft XML Core Services (MSXSL.exe)
- CM0130 - Disable or Restrict Microsoft Build Engine (MSBuild.exe)
- CM0131 - Disable or Restrict Microsoft Management Console (MMC.exe)
- CM0132 - Disable or Restrict InstallUtil

## References

- The Malware Hiding in Your Windows System32 Folder: Mshta, HTA, and Ransomware | <https://www.varonis.com/blog/living-off-the-land-lol-with-microsoft-part-ii-mshta-hta-and-ransomware>
- Applications that can bypass WDAC and how to block them | <https://learn.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/microsoft-recommended-block-rules>
- Mshta | <https://redcanary.com/threat-detection-report/techniques/mshta/>
- How to execute a Local File using HTML Application? | <https://www.codeproject.com/Articles/113678/How-to-execute-a-Local-File-using-HTML-Application>
- HTML Application (HTA) - A GUI for your scripts on Windows | <https://medium.com/@anht_59851/html-application-hta-a-gui-for-your-scripting-on-windows-bfaacf2c3bdd>
- Associate .HTA & various Script File Extensions with Notepad \#2 | <https://github.com/jymcheong/OpenEDRclient/issues/2>
- Detect suspicious Mshta usage | <https://github.com/microsoft/Microsoft-365-Defender-Hunting-Queries/blob/master/Execution/detect-suspicious-mshta-usage.md>
