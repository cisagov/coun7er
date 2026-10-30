# Monitor PowerShell Program

* **ID:** CM0031
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active
* **Nice Work Role:** Defensive Cybersecurity (PD-WRL-001)
* **Skills & Knowledge:** PowerShell administration; logging and script monitoring

## Intended Outcome

Monitoring PowerShell detects adversary execution using PowerShell.

## Introduction

PowerShell is one of the top post-exploitation techniques identified in
cyber incidents. Adversaries use PowerShell to carry out a wide-variety
of malicious tasks.

## Preparation

- PowerShell 5.0 requires at least Windows 7-SP1, Server 2008-R2-SP1, or later.
- PowerShell requires .NET Framework 4.5 or later.
- Determine which logging features should be turned on, based on the size of the resulting log files (to avoid issues with excessively large log files).

## Risks

- This countermeasure may result in excessive resource use.
- Module and Script Block Logging can create large log files.

## Guidance

The enhanced logging features of PowerShell 5.0 must be configured via Group Policy or registry keys. The following log types should be enabled:

- Enable module logging to capture command-line parameters and the content of pipeline output.  
- Enable script block logging to capture the commands inside PowerShell script blocks.  Includes encoded/obfuscated and decoded/deobfuscated code.  Does not capture command output.  
    - To avoid excessive logging, do not enable script block invocation start/stop events as it can result in an excessive number of events. 
- Enable transcription logging to capture records of every PowerShell session, including all input and output.  Transcription will log all commands, including base-64 encoded script blocks in memory. 
    - Consider recording timestamps.
    - Consider writing transcripts to a remote, write-only network share to ensure defenders are reviewing logs in secure manner. 

## Associated Techniques

- [T1059.001](https://attack.mitre.org/techniques/T1059/001) - PowerShell
- [T1070.003](https://attack.mitre.org/techniques/T1070/003) - Indicator Removal: Clear Command History

## Related Countermeasures

- CM0057 - Verify Signing of AppleScripts
- CM0068 - Audit and Restrict Exchange ApplicationImpersonation Role
- CM0096 - Prevent and Detect Attempts to Copy NT Directory Services Database Information Table (NTDS.dit)
- CM0097 - Detect Attempts to Delete Shadow Copies
- CM0110 - Detect Anti-Malware Scan Interface (AMSI) Bypass Attempts
- CM0123 - Remove Malicious Domain Trust from Tenant
- CM0125 - Reset and Monitor adminCount Attribute

## References

- What is Windows PowerShell? | <https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/overview>
- about_Script_Blocks | <https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_script_blocks>
- Greater Visibility Through PowerShell Logging | <https://cloud.google.com/blog/topics/threat-intelligence/greater-visibility/>
