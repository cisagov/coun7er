# Block Untrusted and Unsigned Processes that Run from USB

* **ID:** CM0115
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Infrastructure Support (PD-WRL-004)
* **Skills & Knowledge:** Windows endpoint security; Defender ASR rules, Microsoft Intune, Group Policy Management

## Intended Outcome

Blocking untrusted and unsigned processes that run from USB blocks lateral movement via malicious execution on hosts via USB.

## Introduction

USB drives are commonly abused by adversaries to achieve initial access or lateral movement. Adversaries copy malware to USB drives and employ social engineering techniques to trick users into executing the malware.  

Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) provides an option to prevent unsigned and untrusted executable files (.exe, .dll, .scr) from running from USB drives and SD cards. The intent of this countermeasure not to prevent initial access but to prevent the replication of malware via USB.        


## Preparation

-	Microsoft Defender for Endpoint with real-time and cloud-delivery protection is enabled.
-	Microsoft Defender is the primary anti-virus solution.
-	No Microsoft Defender component is more than two versions old.  

## Risks

-	ASR rules should be initially configured in “Audit Mode” to minimize the likelihood of operational disruption. If time permits, gather and assess telemetry data to ensure the rule will not impact operations.
-	Consider and specify exclusions to minimize operational impact.  

## Guidance

### Block via Group Policy

-	Open the Group Policy Management Console.
-	Create or edit a group policy object.
-	Select `Group Policy Management Editor` browse to `Computer Configuration` and select `Administrative Templates`.
-	Expand `Windows Components>Microsoft Defender Antivirus>Microsoft Defender Exploit Guard>Attack Surface Reduction`.
-	Select and enable `Configure Attack Surface Reduction Rules`.
-	`Show` and enter the rule ID (GUID) in the `Value Name` and specify the value as follows:
    - 1: Block (Enable)
    - 2: Audit (Evaluate)
-	Consider enabling `Audit` mode and monitoring to reduce the likelihood of operational disruption.
-	Apply the policy.

### Block via Intune

-	Sign into the Microsoft Intune Admin Center.
-	Select `Endpoint Security`.
-	Navigate to `Attack Surface Reduction>Create Policy`.
-	Navigate to `Create a Profile`.
-	Under `Platform`, select `Windows 10 and later`. Under `Profile`, select `Attack Surface Reduction Rules`.
-	Select `Create`.
-	On the `Basics` tab, name the policy and enter a description.
-	Select `Configuration Settings`.
-	Configure the `Block Untrusted and Unsigned Processes that Run from USB` policy to `Block`, `Audit`. 
-	Select `Next`.
-	Add scope tags (optional).
-	Under `Assignments`, select `All Devices` or select a specific group. Consider testing the policy on a limited basis prior to applying to all devices. 
-	`Create` the policy.

## Associated Techniques

-	[T1091](https://attack.mitre.org/techniques/T1091/) – Replication Through Removable Media
-   [T1092](https://attack.mitre.org/techniques/T1092/) - Communication Through Removable Media
-   [T1052.001](https://attack.mitre.org/techniques/T1052/001/) - Exfiltration Over Physical Medium: Exfiltration over USB

## Related Countermeasures

-    CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0114 - Enable Microsoft Conditional Access Policy Templates
- CM0119 - Disable Autoplay

## References

- Using Caution with USB Drives | <https://www.cisa.gov/news-events/news/using-caution-usb-drives>
