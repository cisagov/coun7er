# Block Windows Management Instrumentation (WMI) Service

* **ID:** CM0013
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; WMI services and remote management

## Intended Outcome

Blocking the Windows Management Instrumentation (WMI) service blocks adversary discovery, persistence, and lateral movement using WMI.

## Introduction

WMI is the infrastructure for management data and operations on Windows-based operating systems. WMI may be used for incident response. For example, the opensource incident
response tool CimSweep relies on CIM/WMI to perform incident response and hunting operations on Windows. By blocking endpoint access via WMI, adversaries will have one less means of moving laterally and execute payloads across the environment.

## Preparation

- Determine the scope of the impact that blocking WMI would have on operations and prepare contingency plan should unintended disruption occur.

## Risks

- This countermeasure may block legitimate activity.
- Blocking WMI may prevent administrators from taking necessary actions, such as performing automated software updates, and may prevent hunting and IR tools from operating.
- Consider the impact on the enterprise environment and the incident response before blocking WMI.

## Guidance

- WMI should be blocked to inhibit communication from workstation to workstation and blocked between workstations and non-Domain Controllers and File Servers.
- The Group Management Console can be used to create a GPO to block WMI remote use. This can be achieved by configuring Windows Firewall to restrict inbound communications for WMI or by assigning a static DCOM port for WMI and blocking it (the default static port in this case is TCP 24158). 

## Associated Techniques

No Associated Techniques content identified.

## Related Countermeasures

- CM0012 - Disable or Restrict Distributed Component Object Model (DCOM) Protocol
- CM0015 - Block Default Screensaver Programs and Monitor SCR Executable Files
- CM0019 - Disable or Restrict Windows Remote Management (WinRM) Protocol
- CM0032 - Revoke and Regenerate SSH Keys
- CM0038 - Remove Windows Management Instrumentation (WMI) Event Subscription
- CM0039 - Monitor Windows Management Instrumentation (WMI) Events
- CM0096 - Prevent and Detect Attempts to Copy NT Directory Services Database Information Table (NTDS.dit)
- CM0097 - Detect Attempts to Delete Shadow Copies

## References

- Windows Management Instrumentation (WMI) Offense, Defense, and Forensics | <https://www.mandiant.com/sites/default/files/2021-09/wp-windows-management-instrumentation.pdf>
- Starting and stopping WMI service | <https://learn.microsoft.com/en-us/windows/win32/wmisdk/starting-and-stopping-the-wmi-service>
- Setting up a Remote WMI Connection | <https://learn.microsoft.com/en-us/windows/win32/wmisdk/connecting-to-wmi-remotely-starting-with-vista>
- Setting Up a Fixed Port for WMI | <https://learn.microsoft.com/en-us/windows/win32/wmisdk/setting-up-a-fixed-port-for-wmi>
- Windows Management Instrumentation | <https://learn.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page>
