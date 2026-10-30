# Disable Visual Basic for Applications (VBA) Macros

* **ID:** CM0056
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Microsoft Office administration; VBA macros and application controls

## Intended Outcome

Disabling macros prevents the execution of Visual Basic for Applications (VBA) in Office documents that could otherwise afford an adversary the opportunity to achieve user-driven execution.    

## Introduction

Macros enable users to use VBA to automate repetitive tasks in Microsoft Office. Macros are widely used but are commonly abused by adversaries to achieve user execution in pursuit of initial access, execution, lateral movement, and persistence.

Though macros are often employed during phishing campaigns, the intent of this countermeasure is to contain a threat that is abusing macros to expand or maintain access, and thereby slow the pace and extent of adversarial advance.  This countermeasure is intended to be stopgap and should be implemented on a short-term basis.  This countermeasure should only be considered if attempts to restrict macros have not been successful.      


## Preparation

-	Attempt to determine the extent to which macros are used within the organization.
-	Inform stakeholders of the potential for operational disruption.
-	Consider long-term alternatives that are less disruptive e.g. restricting macros.

## Risks

Disabling macros writ-large can disrupt operations. Consider less disruptive countermeasures e.g. restricting macros to minimize the risk of operational disruption.  

## Guidance

### Disable via GPO 

-	If you are not already managing Microsoft Office settings using Group Policy, [import](https://learn.microsoft.com/en-us/troubleshoot/windows-client/group-policy/create-and-manage-central-store) the [Office administrative templates (ADMX)](https://www.microsoft.com/en-us/download/details.aspx?id=49030) for the correct version of Office.
-	Via the Group Policy Management Console, create or edit a Group Policy Object. 
-	Select `Administrative Templates>Microsoft Word 20xx>Word Options>Security Trust Center`.
-	Enable `Disable VBA for Office applications` and apply the policy.
-	Enable `Disable all macros with notification`. 

### Disable via Intune Policy

-	Sign into the Intune Endpoint Manager portal.
-	Select `Devices>Windows>Configuration Profiles>Create Profile`.
-	Select `Platform>Windows xx>Profile>Profile Type>Settings Catalog` Create. 
-	On the `Basics` tab, Name the policy and enter a description.
-	In `Configuration Settings` select `Add Settings`.
-	Search for `Disable VBA for Office applications`.
-	Enable and apply the policy.
-	Search for `Disable all macros with notification`.
-	Enable and apply the policy.

## Associated Techniques

-	[T1204.001](https://attack.mitre.org/techniques/T1204/001/) – User Execution: Malicious Link
-	[T1137.001](https://attack.mitre.org/techniques/T1137/001/) – Office Application Startup: Office Template Macros

## Related Countermeasures

- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0005 - Monitor or Block Microsoft HTML Application (MSHTA) Utility or Restrict HTML Application (HTA) Network Access
- CM0045 - Disable or Monitor Execution of WMIC.exe
- CM0047 - Block High-risk File Types at Web Gateway
- CM0113 - Disable Microsoft Office Add-ins
- CM0116 - Restrict Visual Basic for Applications (VBA) Macros

## References

- Macro Security for Microsoft Office | <https://www.ncsc.gov.uk/guidance/macro-security-for-microsoft-office>
- Small Business Cloud Security Guides: Technical Example – Configure Macro Settings 
- Managing the Central Store for Group Policy Administrative Templates | <https://learn.microsoft.com/en-us/troubleshoot/windows-client/group-policy/create-and-manage-central-store>
- Administrative Templates (ADMX/ADML) for Microsoft Office | <https://www.microsoft.com/en-us/download/details.aspx?id=49030>
