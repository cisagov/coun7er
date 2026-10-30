# Disable or Monitor Windows Script Host (WSH) Administration Tool

* **ID:** CM0078
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; script hosts and application control

## Intended Outcome

Disabling or monitoring the Windows Script Host (WSH) administration tool blocks or detects adversary execution of script-based malware via WSH.

## Introduction

Windows Script Host provides scripting abilities comparable to batch files, and enables execution of VBScript and JScript. If other scripting engines are installed, such as Ruby or Python, WSH can run their code as well. The scripts can be run from the desktop using the WScript.exe program or from a command line using CScript.exe. PowerShell is a substitute for WSH.

## Preparation

* Determine whether WScript.exe or CScript.exe are needed in the environment.

## Risks

* On some systems, disabling WSH may cause problems with system updates and the system recovery mechanism.

## Guidance

WSH should be disabled if it is acceptable from an operations perspective. Otherwise, WSH should be monitored. 

### Disabling WSH

#### Modify the Registry Manually

1. Open the system registry in edit mode
2. Navigate to **HKEY_CURRENT_USER\\Software\\Microsoft\\Windows Script Host\\Settings\\**
3. If it doesn't already exist, create a REG_DWORD called **Enabled**; assign the REG_DWORD the value **0** (zero)
4. Navigate to **HKEY_LOCAL_MACHINE\\Software\\Microsoft\\Windows Script Host\\Settings\\**
5. If it doesn't already exist, create a REG_DWORD called **Enabled**; assign the REG_DWORD the value **0** (zero)

#### Modify via Group Policy Management Console

Registry settings as defined above can be applied across an organization or domain via the Group Policy Management console. Please see [ref] for details.

### Monitoring WSH

Configure the host-based agent to monitor for process execution, command-line parameter inspection, and/or process lineage monitoring. These measures could provide insight into WScript.exe and CScript.exe abuse.

## Associated Techniques

* [T1059.005](https://attack.mitre.org/techniques/T1059/005/) - Command and Scripting Interpreter: Visual Basic
* [T1059.007](https://attack.mitre.org/techniques/T1059/007/) - Command and Scripting Interpreter: JavaScript

## Related Countermeasures

No Related Countermeasures content identified.

## References

- cscript | <https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/cscript>
- wscript | <https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/wscript>
- Disable Windows Script Host (WSH) to block .VBS malware | <https://www.linkedin.com/pulse/disable-windows-script-host-wsh-block-vbs-malware-valerio-de-sanctis>
