# Disable Autoplay

* **ID:** CM0119
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; AutoPlay settings and removable media controls, Group Policy Management

## Intended Outcome

Disabling Autoplay prevents the spread of malware via removable media. 

## Introduction

Windows Autoplay enables files to be executed automatically upon the insertion of removable media into a host. This presents an opportunity for malware to be introduced to, or proliferated on, a network. It is important to note that disabling AutoPlay will prevent the automatic execution of files from removable media but will not prevent users from manually executing said files.   

## Preparation

-	Consider advising users of the change in policy. 

## Risks

No Risks content identified. 

## Guidance

GPO
-	Via the Group Policy Management Console, create or edit a Group Policy Object.
-	Browse to `Computer Configuration>Policies>Administrative Templates>Windows Components>AutoPlay Policies.
-	Under `AutoPlayPolicies`, disable `AutoPlay` and set options to `All Drives`. 
-	Enable `Set the Default Behavior for AutoRun` and set default behavior to `Do Not Execute Any AutoRun Commands`.  
-	Apply the policy.

Intune
-	Select `Device Configuration>Profiles` and choose an endpoint protection profile or create a new one.
-	For the profile type, select `Templates` then `Administrative Templates`.  Name and describe the policy in the policy wizard.
-	Search for `AutoPlay` in the search bar and select `Turn off autoplay`. 
-	Select `Enable` and specify application to `All Drives`. 
-	Apply the policy.

## Associated Techniques

-	[T1091](https://attack.mitre.org/techniques/T1091/) – Replication Through Removable Media

## Related Countermeasures

-	 CM0115 – Block Untrusted and Unsigned Processes that Run from USB

## References

-	Using Caution with USB Drives | <https://www.cisa.gov/news-events/news/using-caution-usb-drives>
-   Quickly Turn Off AutoPlay Using Intune | <https://www.anoopcnair.com/turn-off-autoplay-using-intune-mem/>
-   How to Turn Off AutoPlay via Windows Group Policy | <https://www.manageengine.com/vulnerability-management/misconfiguration/os-security-hardening/how-to-turn-off-autoplay-in-windows-via-group-policy.html>
