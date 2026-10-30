# Block or Monitor Mavinject.exe Utility

* **ID:** CM0008
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; process injection controls and application monitoring

## Intended Outcome

Blocking or monitoring execution of the Mavinject.exe utility (Microsoft
Application Virtualization Injector) blocks or detects adversary defense
evasion using proxy execution of code.

## Introduction

Mavinject.exe is the Microsoft Application Virtualization Injector, a Windows utility that can inject code into external processes as part of the Microsoft Application Virtualization (App-V) client for virtualization. While Mavinject.exe can be used by other legitimate programs, it is primarily used by App-V. If App-V capabilities are not required, it may be safer to remove the Mavinject.exe program and implement defenses to prevent its future installation and execution. 

## Preparation

- Identify a baseline of behavior for Mavinject.exe.
- Determine necessity of permitting Mavinject.exe in environment.

## Risks

- This countermeasure can break legitimate functionality. However, this risk is only likely if App-V is in use in the enterprise.

## Guidance

### Monitor

Configure the host-based agent to monitor for:

- Process execution
- Command-line parameter inspection
- Process lineage monitoring

To gain insight into attempts at binary masquerading, consider collecting process metadata. Adversaries have been observed renaming binaries to evade brittle detections.

- Collect data on module loads and monitor DLLs.
- Analytics can be written to monitor for API calls indicative of process injection. Take care to tune the analytic to reduce false positives.
- Implement network monitoring to detect anomalous network connections and support the ability to correlate connections to processes.

### Block

- Explore the feasibility of removing the Mavinject.exe binary.
- Scanning for filenames and hashes associated with Mavinject.exe and auditing command line/PowerShell logs can be useful for detecting Mavinject.exe. 
- EDR rules can be used to detect and prevent the execution of Mavinject.exe where contextually relevant.
- Mavinject.exe execution can be blocked by leveraging commercial or open-source tools that are designed to block programs from executing (e.g., Windows Defender Application Blocking, CrowdStrike).

## Associated Techniques

- [T1218.013](https://attack.mitre.org/techniques/T1218/013/) - System Binary Proxy Execution: Mavinject

## Related Countermeasures

- CM0005 - Monitor or Block Microsoft HTML Application (MSHTA) Utility or Restrict HTML Application (HTA) Network Access
- RM0033 - Locker to Audit and/or Block Unsigned Applications (*future*)
- CM0046 - Block Execution of Compiled HTML (CHM) Files
- CM0091 - Prevent and Detect System Binary Proxy Execution
- CM0117 - Disable or Monitor Connection Manager Profile Installer (CMSTP.exe)
- CM0127 - Disable Microsoft XML Core Services (MSXSL.exe)
- CM0130 - Disable or Restrict Microsoft Build Engine (MSBuild.exe)
- CM0131 - Disable or Restrict Microsoft Management Console (MMC.exe)
- CM0132 - Disable or Restrict InstallUtil

## References

- mavinject.exe Functionality Deconstructed | <https://posts.specterops.io/mavinject-exe-functionality-deconstructed-c29ab2cf5c0e>
- The Curious Case Of Mavinject.Exe | <https://fourcore.io/blogs/mavinject-curious-process-injection>
