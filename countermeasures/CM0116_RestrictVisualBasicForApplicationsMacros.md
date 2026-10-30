# Restrict Visual Basic for Applications (VBA) Macros

* **ID:** CM0116
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Microsoft Office administration; VBA macro policies and execution restrictions, Group Policy Objects

## Intended Outcome

Restricting macros blocks adversary persistence and lateral movement using malicious Visual Basic for Applications (VBA) macros.

## Introduction

Macros are embedded code written in Visual Basic for Applications (VBA) that enable users to automate repetitive tasks in Microsoft Office applications. Macros are abused by adversaries to achieve user execution in pursuit of initial access, execution, lateral movement, and persistence. 

Though macros are often employed in pursuit of initial access, the intent of this countermeasure is to contain a threat that is leveraging macros to achieve lateral movement or persistence, and thereby slow the pace and extent of adversarial advance.     

## Preparation

-	Attempt to determine the extent to which macros are used within the organization.
-	Assess options for restricting macros.
-	Inform stakeholders of the potential for operational disruption. 

## Risks

Restricting macros can disrupt operations. Pursue restrictions from this frame of reference.  Restrictions should be tested thoroughly and enabled with caution.  

## Guidance

By default, macros from the internet are disabled by Microsoft Office via Mark of The Web (MOTW). This is a decent first line of defense against malicious macros. However, adversaries are capable of bypassing MOTW to get macros to run as intended.

### Restrict via Group Policy Object (GPO)

-	If you are not already managing Microsoft Office settings using Group Policy, [import](https://learn.microsoft.com/en-us/troubleshoot/windows-client/group-policy/create-and-manage-central-store) the [Office administrative templates (ADMX)](https://www.microsoft.com/en-us/download/details.aspx?id=49030) for the correct version of Office.
-	Via the Group Policy Management Console, create or edit a Group Policy Object. 
-	Select `Administrative templates>Microsoft Word 20xx>Word options>Security Trust Center` and consider the following restrictions:
    - Enable `Block macros from running in Office files from the Internet`.
    - `Disable all macros except digitally signed macros`.
    - Disable `Allow mix of policy and user locations`.
    - Enable `Automation Security`.
-	You may also consider only permitting the execution of macros to certain groups that require the need to do so to support an operational requirement.  


### Restrict via Intune Policy

-	You may use Intune cloud policy or Group Policy ADMX templates to restrict macros.
-	Sign into the Intune Endpoint Manager portal.
-	Select `Devices>Windows>Configuration Profiles>Create Profile`.
-	Select `Platform>Windows xx>Profile>Profile Type>Settings Catalog’ Create.` 
-	On the `Basics` tab, Name the policy and enter a description.
-	`Configuration Settings` select `Add Settings`.
-	Search for macro related policies e.g. `macros from running in Office files from the internet`.
-	Select and enable the appropriate policies.

### Restrict Macros via Microsoft Attack Surface Reduction (ASR) Rules in Intune

-	Sign into the Microsoft Intune Endpoint Management portal.
-	Set platform to `Windows 10, Windows 11, and Windows Server`.
-	Set profile to `Attack Surface Reduction Rules`.
-	Set the name and enter a description.
-	Consider the following rules:
    - `Block Win32 API calls from Office`
    - `Block execution of potentially obfuscated scripts`
    - `Block JavaScript or VBScript from launching downloaded executable content`
-	Add inclusions and exclusions.
-	Create the policy.

### Additional Considerations

-	Ensure the continual logging and monitoring of macro execution.
-	Implement an application control solution and configure it to scrutinize macros.
-	Implement email and web content filtering to inspect Microsoft Office files for macros.
-	End user security training.
-	Prevent users from being able to modify security settings. 

## Associated Techniques

-	[T1204.001](https://attack.mitre.org/techniques/T1204/001/) – User Execution: Malicious Link
-	[T1137.001](https://attack.mitre.org/techniques/T1137/001/) – Office Application Startup: Office Template Macros

## Related Countermeasures

- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0047 - Block High-risk File Types at Web Gateway
- CM0056 - Disable Visual Basic for Applications (VBA) Macros
- CM0113 - Disable Microsoft Office Add-ins

## References

- Macro Security for Microsoft Office | <https://www.ncsc.gov.uk/guidance/macro-security-for-microsoft-office>
- Essential Eight Configure Microsoft Office Macro Settings | <https://learn.microsoft.com/en-us/compliance/essential-eight/e8-macro>
- Restricting Microsoft Office Macros | <https://www.cyber.gov.au/sites/default/files/2023-11/PROTECT%20-%20Restricting%20Microsoft%20Office%20Macros%20%28November%202023%29.pdf>
- Macros from the internet are blocked by default in Office | <https://learn.microsoft.com/en-us/deployoffice/security/internet-macros-blocked>
- Mark Of The Web Bypass | <https://redcanary.com/threat-detection-report/techniques/mark-of-the-web-bypass/>
- Managing the Central Store for Group Policy Administrative Templates | <https://learn.microsoft.com/en-us/troubleshoot/windows-client/group-policy/create-and-manage-central-store>
- Administrative Templates (ADMX/ADML) for Microsoft Office | <https://www.microsoft.com/en-us/download/details.aspx?id=49030>
