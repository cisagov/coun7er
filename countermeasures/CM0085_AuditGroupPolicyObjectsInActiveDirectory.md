# Audit Group Policy Objects (GPOs) in Active Directory (AD)

* **ID:** CM0085
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active
* **Nice Work Role:** Vulnerability Analysis (PD-WRL-007)
* **Skills & Knowledge:** Active Directory administration; Group Policy and configuration auditing

## Intended Outcome

Auditing Group Policy Objects (GPOs) in Active Directory (AD) restricts adversary persistence, privilege escalation, and execution.

## Introduction

A Group Policy Object (GPO) is a virtual collection of policy settings that is enforced on devices and users in an Active Directory (AD) environment. Adversaries can abuse GPOs by identifying writable GPOs to perform privilege escalation, modifying security settings to perform specific tasks, and distributing malware. 
During an incident, audit logs should be checked to determine who (device and/or user) performed what actions. This will allow responders to triage and isolate rogue or suspicious users and devices. Both Microsoft’s Policy Analyzer and Backup-GPO can be used for quickly restoring policy settings to a known secure baseline. 

## Preparation

- Consult documentation of last known good or approved configurations.

## Risks

No risks content identified.

## Guidance

Audits should prioritize top level domain policies, tier 0 assets, and domains with suspicious or anomalous activity. The rest of this guidance will cover how to enable Active Directory's audit policy setting and how to compare GPOs to a secure baseline. 

### Enable Audit Policy Setting and Review System Event Logs Concerning GPOs

Active Directory's audit policy settings define which system events and account activity are tracked. 
An audit policy in Active Directory defines what events to keep track of.

The events that are tracked will ultimately determine what information can be reviewed or not. The following steps were provided in a Microsoft Q&A response for how to enable GPO audits. 

Step 1: Enable Audit

1. Launch “Group Policy Management Console”, create a new GPO and link to Domain Controllers OU.

2. From the context menu, click on “Edit” to open the “Group Policy Management Editor” window. Navigate to “Computer Configuration” -> “Policies” -> “Windows Settings” -> “Security Settings” -> “Advanced Audit Policy Configuration” -> “Audit Policies” -> “DS Access”.

3. Click “Audit Directory Service changes”, click “Configure the following audit events” and choose “Success”.

4. To force Group Policy Update, we could run “gpupdate /force” on the domain controllers.

Step 2: Configuring Group Policy Container Objects auditing

1. Launch the ADSIEdit.msc and Connect to the Default naming context.

2. Navigate to CN=Policies,CN=System,DC=domain -> Open the “Properties of Policies” object -> Go to the Security tab -> Click the Advanced button.

3. Go to the Auditing tab -> Add the Principal Everyone -> Choose the Type Success -> For Applies to, click This object and Descendant objects -> Under Permissions, select following checkboxes: “Create groupPolicyContainer objects”, “Delete”, “Modify Permissions” -> Click OK.

Step 3: Review changes in the security Event Log. 

1. Event ID 5137 captures who created the GPO.

2. Event ID 5141 is logged with the Unique ID of the GPO that was deleted and the user who performed the deletion. 

3. Event ID 5136 is generated when a GPO is modified. 

### Compare GPO Configurations to Secure Baseline

#### Comparing Against Audit Policy Baseline

Another approach would be to have a baseline and backups of approved GPO policies to perform manual audits. Backup-GPO can be used to create a backup of one or more GPOs. Get-GPOReport can then be used to grab the current GPO and compare it against a secure baseline. Regular backups would need to be performed to have the most current and approved GPO.

#### Microsoft's Security Compliance Toolkit

Microsoft’s Security Compliance Toolkit (SCT) is a set of tools for enterprise administration. Within the toolkit, the Policy Analyzer enables administrators to capture a baseline and compare it to different versions for the purposes of identifying where changes occurred. Example usage and screen grabs of the policy analyzer and other tools in the toolkit can be viewed in the "Microsoft Security Compliance Toolkit" article provided in the references.

## Associated Techniques

- [T1484.001](https://attack.mitre.org/techniques/T1484/001) - Domain Policy Modification: Group Policy Modification
- [T1484.002](https://attack.mitre.org/techniques/T1484/002) - Domain Policy Modification: Domain Trust Modification

## Related Countermeasures

- CM0060 - Restrict and Monitor Domain Accounts with Local Administrator Access to Workstations
- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts
- CM0113 - Disable Microsoft Office Add-ins
- CM0125 - Reset and Monitor adminCount Attribute
- CM0133 - Reset Access Control Lists to Default Values
- CM0140 - Configure Windows Audit Policy
- CM0144 - Audit SYSVOL for Vulnerable Group Policy Preferences

## References

- Group Policy Objects | <https://learn.microsoft.com/en-us/previous-versions/windows/desktop/policy/group-policy-objects>
- How to Audit Group Policy Changes Using Event Logs | <https://www.lepide.com/how-to/audit-changes-made-to-group-policy-objects.html>
- Top 11 Windows Audit Policy Best Practices | <https://activedirectorypro.com/audit-policy-best-practices/>
- Backup-GPO | <https://learn.microsoft.com/en-us/powershell/module/grouppolicy/backup-gpo?view=windowsserver2022-ps>
- Microsoft Security Compliance Toolkit - How to use | <https://learn.microsoft.com/en-us/windows/security/operating-system-security/device-management/windows-security-configuration-framework/security-compliance-toolkit-10>
- GPO Abuse | <https://redfoxsec.com/blog/gpo-abuse/>
