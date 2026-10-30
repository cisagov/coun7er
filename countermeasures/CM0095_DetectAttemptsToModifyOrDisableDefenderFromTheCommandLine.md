# Detect Attempts to Modify or Disable Defender from the Command Line 

* **ID:** CM0095
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active
* **Nice Work Role:** Defensive Cybersecurity (PD-WRL-001)
* **Skills & Knowledge:** Microsoft Defender administration; command-line monitoring and tamper protection

## Intended Outcome

Detecting attempts to abuse command line utilities to modify or disable Windows Defender alerts responders to attempts to evade defenses.

## Introduction

Adversaries modify or disable security tools to evade detection and impede response.  PowerShell and CMD can be used to disable or modify firewalls, antivirus solutions, and logging.

## Preparation

- Ensure Tamper protection is enabled for Microsoft Defender.  Tamper protection prevents settings from being modified or disabled. Tamper Protection does not prevent an adversary from creating exclusions to prevent Microsoft Defender from scanning specific files or directories.  
- Ensure comprehensive endpoint detection and response.  Logs should be aggregated and processed in a centralized location to enable analysis and response efforts.  

## Risks

No Risks content identified.

## Guidance

Monitor process creation, specifically PowerShell and CMD.  If possible, consider monitoring for instances in which the processes of security utilities are terminated.  File creation can also indicate attempts to modify the configuration files of security tools.    

-	Modify (Add Exclusions)
    - Monitor the PowerShell and CMD processes.
    - Monitor the command line for the Add-MpPreference cmdlet and the following file extensions (.exe, .dll, .vbs, .zip, .bat, .ps1).
-	Disable Defender
    - Monitor the PowerShell and CMD processes.
    - Monitor the command line for the Set-MpPreference cmdlet and the MpCmdRun utility and the following parameters (disablerealtimemonitoring, disableioavprotection, disablebehaviormonitoring, disableintrusionpreventionsystem, exclusionprocess, disablescriptscanning).

## Associated Techniques

-	[T1562.001](https://attack.mitre.org/techniques/T1562/001/) – Impair Defenses: Disable or Modify Tools
-	[T1562.004](https://attack.mitre.org/techniques/T1562/004/) – Impair Defenses: Disable or Modify System Firewall

## Related Countermeasures

- CM0110 - Detect Anti-Malware Scan Interface (AMSI) Bypass Attempts

## References

- Disable or Modify Tools | <https://redcanary.com/threat-detection-report/techniques/disable-or-modify-tools/>
- Defender Endpoint Prevent Changes to Security Settings with Tamper Protection | <https://learn.microsoft.com/en-us/defender-endpoint/prevent-changes-to-security-settings-with-tamper-protection?view=o365-worldwide>
- Disable and Bypass Defender | <https://viperone.gitbook.io/pentest-everything/everything/everything-active-directory/defense-evasion/disable-defender>
