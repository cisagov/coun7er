# Disable and Monitor Regasm.exe and Regsvcs.exe Utilities

* **ID:** CM0006
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; .NET utilities and application control


## Intended Outcome

Disabling or monitoring registration utilities blocks or detects adversary defense evasion using proxy execution of code.

## Introduction

Registration utilities include Regasm.exe (Assembly Registration Tool) and Regsvcs.exe (.NET Services Installation Tool). Regasm.exe reads the metadata within an assembly and adds the necessary entries to the registry, which allows COM clients to create .NET Framework classes transparently.

## Preparation

- Determine necessity of permitting REGASM.exe and REGSVCS.exe in environment.
- Identify a baseline of behavior of the two programs.

## Risks

- This countermeasure can break legitimate functionality.
- Disabling REGASM.exe and REGSVCS.exe may break some legitimate functionality.
- An analysis of how REGASM.exe and REGSVCS.exe are legitimately used within an organization's environment can help fine-tune rules and detections.

## Guidance

### Monitor

Configure the host-based agent to monitor for:
- Process execution
- Command-line parameter inspection
- Process lineage monitoring 

To gain insight into attempts at binary masquerading, consider collecting process metadata Adversaries have been observed renaming binaries to evade brittle detections.
- Monitor for network calls from registration utility binaries
- Monitor DLL's

### Block 

- Scanning for filenames and hashes associated with REGASM.exe and REGSVCS.exe 
- Auditing command line/PowerShell logs can be useful for detecting usage of these two applications
- EDR rules can be used to detect and prevent the execution of REGASM.exe and REGSVCS.exe where contextually relevant
- Configure the host-based agent to block the use of registration utilities and deny them the ability to make network calls

## Associated Techniques

-   [T1218.009](https://attack.mitre.org/techniques/T1218/009) - System Binary Proxy Execution: Regsvcs/Regasm

## Related Countermeasures

- CM0005 - Monitor or Block Microsoft HTML Application (MSHTA) Utility or Restrict HTML Application (HTA) Network Access
- CM0007 - Disable and Monitor Verclsid.exe System Binary
- CM0046 - Block Execution of Compiled HTML (CHM) Files
- CM0091 - Prevent and Detect System Binary Proxy Execution
- CM0117 - Disable or Monitor Connection Manager Profile Installer (CMSTP.exe)
- CM0127 - Disable Microsoft XML Core Services (MSXSL.exe)
- CM0130 - Disable or Restrict Microsoft Build Engine (MSBuild.exe)
- CM0131 - Disable or Restrict Microsoft Management Console (MMC.exe)
- CM0132 - Disable or Restrict InstallUtil

## References

- Threat Hunting for the Most Common MITRE ATT&CK Techniques (Part 4) | <https://medium.com/axon-technologies/threat-hunting-for-the-most-common-mitre-att-ck-techniques-part-4-72e4fc8178bc>
- Regasm.exe (Assembly Registration Tool) | <https://learn.microsoft.com/en-us/dotnet/framework/tools/regasm-exe-assembly-registration-tool>
- Regsvcs.exe | <https://lolbas-project.github.io/lolbas/Binaries/Regsvcs/>
- Agent Tesla: Old RAT Uses New Tricks to Stay on Top | <https://www.sentinelone.com/labs/agent-tesla-old-rat-uses-new-tricks-to-stay-on-top/>
- metasploit-framework applocker_evasion_regasm_regsvcs.md | <https://github.com/rapid7/metasploit-framework/blob/master/documentation/modules/evasion/windows/applocker_evasion_regasm_regsvcs.md>
