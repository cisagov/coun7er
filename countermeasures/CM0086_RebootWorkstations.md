# Reboot Workstations

* **ID:** CM0086
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Workstation administration; service availability and safe reboot procedures

## Intended Outcome

Rebooting workstations removes memory-resident malware from Random Access Memory (RAM). 

## Introduction

Adversaries may deploy memory-resident malware to minimize their presence in a target network, circumvent defenses, and reduce the risk of being detected. Memory-resident malware exists in RAM and does not typically involve leaving artifacts on a host’s hard-drive.    

## Preparation

- Rebooting a workstation will destroy memory-resident malware and its artifacts. This can ultimately hinder further analysis. If this is a concern, consider capturing a memory dump before rebooting the host. 

## Risks

- Rebooting a workstation will clear memory-resident malware from RAM. If the malware has instantiated a persistence mechanism e.g. WMI event subscriptions, scheduled tasks, etc., the malware will persist after reboot. As such, rebooting workstations may not be adequate to completely mitigate the threat. 
- If a workstation is infected with ransomware, a reboot can result in permanent data loss.

## Guidance

-	Notify users and stakeholders in advance of any planned system reboots to minimize any potential disruptions or downtime. 
-	Consider capturing artifacts before the reboot for later analysis.
-	After the system has been rebooted, continually monitor the workstation for indications of persistence. 

## Associated Techniques

-	[T1055](https://attack.mitre.org/techniques/T1055/) – Process Injection
-	[T1112](https://attack.mitre.org/techniques/T1112/) – Modify Registry 

## Related Countermeasures

- CM0066 - Update Firmware
- CM0067 - Install Critical Software Security Updates
- CM0026 - Reboot Servers

## References

- What is Fileless Malware | <https://www.fortinet.com/resources/cyberglossary/fileless-malware>
