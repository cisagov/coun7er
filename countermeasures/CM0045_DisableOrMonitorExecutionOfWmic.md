#  Disable or Monitor Execution of WMIC.exe

* **ID:** CM0045
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; WMIC and command-line application control

## Intended Outcome

Disabling or monitoring execution of wmic.exe will either prevent or alert to adversaries executing local or remote commands for lateral movement or persistence. 

## Introduction

Windows Management Instrumentation (WMI) is a Microsoft module that is defined as the infrastructure for management data and operations. WMI is used to automate tasks, execute processes and scripts, and system configuration. WMIC.exe is a command-line tool to access WMI without the use of scripts. 

## Preparation

- Identify applications that require WMI to function properly. 

## Risks

- Disabling WMI could break Microsoft applications that depend on WMI to function. 

## Guidance

### Disabling Execution of WMIC.exe

WMI can be disabled by enabling Attack Surface Reduction (ASR) rules to prevent processes created by WMI commands from running. 

Another approach is to block wmic.exe directly using application control. Windows Defender Application Control (WDAC) policy rules can be configured to block wmic.exe. 

### Monitoring Execution of WMIC.exe

* Process Monitoring: Monitor for new processes and/or commands containing "wmi".
    * Windows Security log Event ID 4688 will track when a new process has been created.
    * Sysmon Event ID 10 tracks process access. 
    * WMI can spawn two different processes: wmic.exe or wmiprvse.exe.
    * A child process, scrcons.exe, is responsible for executing VBscript and Jscript code when ActiveScriptEventConsumer class is leveraged for persistence. 
    
* Network Activity: Remote WMI is handled by remote services such as distributed component object model (DCOM) and windows remote management (WinRM).
    * WMI over DCOM uses Remote Procedure Call (RPC) over port 135.
    * WMI over WinRM uses HTTP port 5985 or HTTPS port 5986. 


* Command Logging/History: PowerShell script block logging and transcription logging should be enabled to view attempts at using wmic through PowerShell. 

## Associated Techniques

- [T1047](https://attack.mitre.org/techniques/T1047/) - Windows Management Instrumentation
- [T1021.006](https://attack.mitre.org/techniques/T1021/006/) - Remote Services: Windows Remote Management 
- [T1546](https://attack.mitre.org/techniques/T1546/) - Event Triggered Execution
- [T1220](https://attack.mitre.org/techniques/T1220/) - XSL Script Processing
- [T1490](https://attack.mitre.org/techniques/T1490/) - Inhibit System Recovery

## Related Countermeasures

- CM0005 - Monitor or Block Microsoft HTML Application (MSHTA) Utility or Restrict HTML Application (HTA) Network Access 
- CM0056 - Disable Visual Basic for Applications (VBA) Macros
- CM0097 - Detect Attempts to Delete Shadow Copies
- CM0138 - Audit and Remove Malicious Udev Rules

## References

- WMI - The Stealthy Component | <https://www.cynet.com/attack-techniques-hands-on/wmi-the-stealthy-component/>
- Windows Management Instrumentation | <https://redcanary.com/threat-detection-report/techniques/windows-management-instrumentation/>
- Windows Management Instrumentation Attacks – Detection & Response | <https://www.socinvestigation.com/windows-management-instrumentation-attacks-detection-response/>
- Windows Management Instrumentation | <https://learn.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page>
- A Description of the Windows Management Instrumentation (WMI) Command-Line Utility (Wmic.exe) | <https://support.microsoft.com/en-us/topic/a-description-of-the-windows-management-instrumentation-wmi-command-line-utility-wmic-exe-f5c16751-3a83-49ee-030d-5092ce1a04bb>
