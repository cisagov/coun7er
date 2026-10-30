# Disable Microsoft Office Add-ins

* **ID:** CM0113
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Microsoft Office administration; add-in management and application controls, Group Policy Objects

## Intended Outcome

Disabling Microsoft (MS) Office add-ins blocks an adversary from maintaining persistence using MS Office Add-ins. 

## Introduction

Adversaries can use malicious add-ins to inject code payloads into Office documents. Adversaries may use malicious Office Ad-ins as a means of avoiding detection by anti-malware solutions that are better at detecting macros or Visual Basic (VBA) scripts. Malicious Office Add-ins can run on application start-up and allow adversaries to maintain persistent access to systems. 

## Preparation

- Identify what Office Add-ins (if any) are necessary to business operations.
- Consider enabling Attack Surface Reduction (ASR) rules blocking Office Add-ins from creating child processes: 
	- Navigate to `Computer Configuration > Policies > Administrative Templates > Windows Components > Microsoft Defender Antivirus > Microsoft Defender Exploit Guard > Attack Surface Reduction`
    - Set `Configure Attack Surface Reduction Rules` to Enabled and set the `D4F940AB-401B-4EFC-AADC-AD5F3C50688A` key to value 1. There are other ASR rules for Office applications, including blocking API calls by macros or blocking executable content that can also be enabled.

## Risks

- Some departments may use Office Add-ins for legitimate purposes and disabling them can affect effectiveness of operations.
- Disabling all Add-ins via the Group Policy Objects (GPOs) may disable all COM add-ins as well if no allowed/managed add-ins are specified.  

## Guidance

### Disabling Add-ins via Group Policy

Update Group Policy for Add-ins.
- Create a GPO object.
- In the editor, navigate to `User Configuration > Policies > Administrative Templates > Microsoft Office`.
- To block all add-ins, enable the `Block all unmanaged add-ins` policy setting.
- If any add-ins are mandatory to business operations, enable the `List of managed add-ins` policy setting. Follow provided GPO instructions to manage any allowed add-ins by adding their ProgIDs (found in the registry under the `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Office\<application>\Addin` or `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\<application>\Addins` keys) into the policy. All add-ins not in the list will be blocked by the previous policy. 

Additional GPO settings and their recommended settings for Office 2016 applications:
- Under `User Configuration > Policies > Administrative Templates > Microsoft <application> 2016 > <application> Options > Security > Trust Center` set `Disable all application add-ins` to enabled. This setting is available for Excel 2016, PowerPoint 2016, Project 2016, Visio 2016, and Word 2016. 

Update group policies on all machines by either using the Group Policy Management Console (GPMC) or the `Invoke-GPUpdate` cmdlet in PowerShell. Firewall settings may need to be altered to allow the following traffic:

| Server Port           | Service                       | Type of network traffic                     |
|-----------------------|-------------------------------|---------------------------------------------|
| TCP RPC dynamic ports | Task Scheduler service        | Remote Scheduled Tasks Management           |
| TCP port 135          | Remote Procedure Call service | RPC Endpoint Mapper (RPC-EPMAP)             |
| all TCP ports         | Winmgmt                       | Windows Management Instrumentation (WMI-in) |

Windows Server offers a starter GPO for configuring all the necessary ports, titled `Group Policy Remote Update Firewall Ports`. Microsoft recommends using that policy as a starting point and editing as needed based on organizational and business needs. After ports are configured, the following steps push GPO updates:
- Via the GPMC, navigate to the organizational unit.
- Click `Group Policy Update...` and select "yes" when the `Force Group Policy update` dialog box pops up.

Monitor changes.
- Monitor changes to the GPO policy for adversary behavior such as disabling policy settings or adding add-ins to the managed list.
- Monitor the `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Office\<application>\Addin` and `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\<application>\Addins` registry keys for any changes.

Add-ins can also be disabled manually on hosts via the application using them. Navigate to `File > Get Add-ins > My Add-ins` and select the Remove option for all Add-ins you wish to remove. 

### Deleting Add-ins

If an Add-in is no longer needed, delete it instead of disabling.
- Navigate to `Integrated Apps` in the admin center.
- In the configuration pane of the selected add-in, go to `Advanced Settings > Add-ins`. 
- Select the correct add-in from the list and remove it. 

## Associated Techniques

- [T1137.006](https://attack.mitre.org/techniques/T1137/006/) - Office Application Startup: Add-ins

## Related Countermeasures

- CM0002 - Enable Email Attachment Filtering and Message Authentication
- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0056 - Disable Visual Basic for Applications (VBA) Macros
- CM0085 - Audit Group Policy Objects (GPOs) in Active Directory (AD)
- CM0116 - Restrict Visual Basic for Applications (VBA) Macros

## References

While some of the references are focused on keeping add-ins enabled, they provide information on disabling or removing them as well. 

- Disable Add-ins | <https://support.microsoft.com/en-us/office/disable-add-ins-71d7505d-1349-4741-980a-8cb65e0097bd>
- Enable Or Disable Microsoft Office Add-ins Via Group Policy | <https://www.kapilarya.com/enable-or-disable-microsoft-office-add-ins-via-group-policy>
- View, manage, and install add-ins for Excel, PowerPoint, and Word | <https://support.microsoft.com/en-us/office/view-manage-and-install-add-ins-for-excel-powerpoint-and-word-16278816-1948-4028-91e5-76dca5380f8d>
- Support for keeping add-ins enabled | <https://learn.microsoft.com/en-us/office/vba/outlook/Concepts/Getting-Started/support-for-keeping-add-ins-enabled#system-administrator-control-over-add-ins>
- Hardening Microsoft 365, Office 2021, Office 2019 and Office 2016 | <https://www.cyber.gov.au/sites/default/files/2023-03/PROTECT%20-%20Hardening%20Microsoft%20365%2C%20Office%202021%2C%20Office%202019%20and%20Office%202016%20%28January%202022%29.pdf>