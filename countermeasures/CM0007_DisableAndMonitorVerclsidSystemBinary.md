# Disable and Monitor Verclsid.exe System Binary

* **ID:** CM0007
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; system binaries and application control


## Intended Outcome

Disabling and monitoring the Verclsid.exe system binary blocks or detects adversary defense evasion using proxy execution of code.

## Introduction

Verclsid.exe is known as the Extension CLSID Verification Host and can be used to verify each shell extension before use by Windows Explorer or the Windows Shell (faulty  shell extensions that can cause crashes when used with explorer.exe). However, the tool can be abused to execute malicious component object model (COM) objects.

## Preparation

- Identify a baseline of behavior for Verclsid.exe.
- Determine necessity of permitting Verclsid.exe in the environment.

## Risks

- This countermeasure can break legitimate functionality.
- Removing Verclsid.exe may result in backward compatibility issues.

## Guidance

### Monitor

Configure the host-based agent to monitor for:
- Process execution
- Command-line parameter inspection
- Process lineage monitoring

To gain insight into attempts at binary masquerading, consider collecting process metadata Adversaries have been observed renaming binaries to evade brittle detections.
- Monitor for network calls from the verclsid.exe binary.

### Block

- Update the Windows Firewall or host-based agent to block network connections from the verclsid.exe binaries

`%SystemRoot%\system32\verclsid.exe`

`%SystemRoot%\syswow64\verclsid.exe`

## Associated Techniques

- [T1218.012](https://attack.mitre.org/techniques/T1218/012/) - System Binary Proxy Execution: Verclsid

## Related Countermeasures

- CM0005 - Monitor or Block Microsoft HTML Application (MSHTA) Utility or Restrict HTML Application (HTA) Network Access
- CM0006 - Disable and Monitor Regasm.exe and Regsvcs.exe Utilities
- CM0046 - Block Execution of Compiled HTML (CHM) Files
- RM0273 - Reduce Number of Specific Systems Able to Access Internet Directly (*future*)
- RM0368 - Isolate Affected Systems by Blocking Access to the Internet (*future*)
- CM0091 - Prevent and Detect System Binary Proxy Execution
- CM0117 - Disable or Monitor Connection Manager Profile Installer (CMSTP.exe)
- CM0127 - Disable Microsoft XML Core Services (MSXSL.exe)
- CM0130 - Disable or Restrict Microsoft Build Engine (MSBuild.exe)
- CM0131 - Disable or Restrict Microsoft Management Console (MMC.exe)
- CM0132 - Disable or Restrict InstallUtil

## References

- Microsoft Security Bulletin MS06-015 - Critical Vulnerability in Windows Explorer Could Allow Remote Code Execution (908531) | <https://learn.microsoft.com/en-us/security-updates/SecurityBulletins/2006/ms06-015>
- How my lack of understanding of how processes exit on Windows XP forced a security patch to be recalled | <https://devblogs.microsoft.com/oldnewthing/20070504-00/?p=26983>
- Verclsid.exe | <https://lolbas-project.github.io/lolbas/Binaries/Verclsid/>
- The missing verclsid.exe documentation | <https://medium.com/falconforce/the-missing-verclsid-exe-documentation-7080757e9acf>
- COM IDs & Registry keys in a nutshell | <https://www.codeproject.com/Articles/1265/COM-IDs-Registry-keys-in-a-nutshell>
- Hancitor | <https://attack.mitre.org/software/S0499/>
- ReaCOM Verclsid.md | <https://github.com/homjxi0e/ReaCOM/blob/master/Classes/Verclsid.md>
- Old Phishing Attacks Deploy a New Methodology: Verclsid.exe | <https://redcanary.com/blog/threat-detection/verclsid-exe-threat-detection/>
