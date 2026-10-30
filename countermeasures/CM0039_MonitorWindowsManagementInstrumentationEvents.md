# Monitor Windows Management Instrumentation (WMI) Events

* **ID:** CM0039
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active
* **Nice Work Role:** Defensive Cybersecurity (PD-WRL-001)
* **Skills & Knowledge:** Security monitoring; WMI logging and event analysis

## Intended Outcome

Monitoring Windows Management Instrumentation (WMI) events detects
adversary execution of malicious commands and payloads.

## Introduction

WMI is the infrastructure for management data and operations on
Windows-based operating systems.

## Preparation

No Preparation content identified.

## Risks

No Risks content identified.

## Guidance

- Collect Information about WMI using PowerShell. Execute the following commands in PowerShell to collect WMI event filters, consumers, and bindings on a system. 
- Collect Information about WMI using free, open source, modular PowerShell-based incident response framework such as Arthur and/or KANSA.
- Collect Information about WMI using Vendor Tools.
    - In addition to the methods described above, various vendor tools may be used to collect WMI events (or indeed are configured to collect them by default). For example, Splunk supports WMI data collection. 
- Configure Sysmon to capture WMI Event Filter, Consumer, and Binding Activity. Sysmon can be configured to monitor WMIEventFilter activity (event 19), WMIEventConsumer activity (event 20), and WMIEventConsumerToFilter activity (event 21). Roberto Rodriguez' (@Cyb3rWard0g) Sysmon configuration file can be installed to capture Event IDs 19, 20, and 21. Sysmon can be installed with this configuration file using the following command.
    - sysmon.exe -i -c .\config_file.xml

## Associated Techniques

- [T1047](https://attack.mitre.org/techniques/T1047) - Windows Management Instrumentation

## Related Countermeasures

- CM0013 - Block Windows Management Instrumentation (WMI) Service
- CM0038 - Remove Windows Management Instrumentation (WMI) Event Subscription
- CM0096 - Prevent and Detect Attempts to Copy NT Directory Services Database Information Table (NTDS.dit)
- CM0097 - Detect Attempts to Delete Shadow Copies

## References

- Windows Management Instrumentation | <https://learn.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page>
- Investigating WMI Attacks | <https://www.sans.org/blog/investigating-wmi-attacks/>
- Kansa: A PowerShell-based incident response framework | <https://powershellmagazine.com/2014/07/18/kansa-a-powershell-based-incident-response-framework/>
- MalwareArchaeology/ARTHIR | <https://github.com/MalwareArchaeology/ARTHIR>
- Monitor data through Windows Management Instrumentation (WMI) | <https://docs.splunk.com/Documentation/Splunk/latest/Data/MonitorWMIdata>
- StartLogging.xml | <https://gist.github.com/Cyb3rWard0g/136481552d8845e52962534d1a4b8664>
