# Remove Windows Management Instrumentation (WMI) Event Subscription

* **ID:** CM0038
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Eliminate
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; WMI event subscriptions and persistence removal

## Intended Outcome

Removing a Windows Management Instrumentation (WMI) event subscription terminates adversary persistence and/or lateral movement via malicious content execution triggered by a WMI event subscription. 

## Introduction

WMI is the infrastructure for management data and operations on Windows-based operating systems. Adversaries create WMI subscriptions to achieve event-initiated arbitrary code execution. This technique is typically employed to achieve and/or persistence.

Note that Microsoft is deprecating WMIC in Windows Server in favor or interacting with WMI via PowerShell in Windows 11 (starting with build 22572).

## Preparation

No Preparation content identified.

## Risks

- This countermeasure can break legitimate functionality.
- Removing a benign WMI Event Subscription may break legitimate applications.

## Guidance

Gain Visibility into WMI Subscription Events

- Configure Sysmon to log WMiEventFilter, WmiEventConsumer, and WMIEventConsumerToFilter to enable detection of WMI abuse. 

Search for and Review WMI Subscription Events

- Use PowerShell or AutoRuns to list WMI Subscription Events.
- Review WMI Subscription Events for suspicious attributes (invocation of scripting interpreter coupled with encoded content, etc.)

Eradicate WMI Subscription Events

- Use PowerShell or AutoRuns to eradication malicious WMI Subscription Events.

## Associated Techniques

- [T1047](https://attack.mitre.org/techniques/T1047) - Windows Management Instrumentation

## Related Countermeasures

- CM0013 - Block Windows Management Instrumentation (WMI) Service
- CM0039 - Monitor Windows Management Instrumentation (WMI) Events
- CM0103 - Deny log on as a batch job
- CM0120 - Remove Cron Job
- CM0138 - Audit and Remove Malicious Udev Rules
- CM0147 - Remove Suspicious Scheduled Tasks

## References

- Windows Management Instrumentation | <https://learn.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page>
- Detecting & Removing an Attacker's WMI Persistence | <https://medium.com/threatpunter/detecting-removing-wmi-persistence-60ccbb7dff96>
- AutoRuns for Windows v14.1 | <https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns>
- Sysinternals Suite | <https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite>
