# Configure Windows Audit Policy

* **ID:** CM0140
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine, Enable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; audit policies and security event logging

## Intended Outcome

Configuring the Windows Audit policy blocks or prevents an adversary from remaining undetected on a network.

## Introduction

Windows Audit Policy describes which events to log in the event viewer, and which criteria must be met for the event to be logged. For example, policy can state that all logon events must be logged, regardless of outcome, or that only failed attempts must be logged. Event logs help administrators and defenders understand what happened on a system before, during, and after an incident. 

Adversaries may edit or change audit policies so that their actions on the system don't cause event logs or alert defenders of unusual system activity. Additionally, event logs may provide defenders with logs about process creation or script execution that antivirus solutions and Security Information and Event Management (SIEM) systems may miss or exclude due to sheer volume. 

## Preparation

- Ensure event logging is enabled in the environment. 
- Ensure appropriate space is allocated for all expected logs. 
- Define collection requirements of events. 
- Decide how audit logs are collected, stored, and analyzed. 

## Risks

- If there is not appropriate space allocated for event logs, additional events may not get logged. Ensure there is enough space for all expected logs. 
- Continuous auditing may affect machine performance. Consider testing policies before applying them to the production environment. 
- More audit logs increases the strain on analysts. Continuously monitor and refine audit policies to ensure maximum efficiency. 
- When advanced audit policy settings are enabled via Group Policy, current or default audit policy settings are cleared. Ensure that all necessary events are still logged and audited after making changes to the policy. 
- Using both basic and advanced policies may cause unexpected results in reporting (advanced policies may override all basic policies). Ensure that only one of the above is enabled. 
- If using advanced policies, Microsoft recommends enabling the `Audit: Force audit policy subcategory settings (Windows Vista or later) to override audit policy category settings` setting to ensure policies are not overridden.

## Guidance

### Audit and Configure Audit Policies

Events that get logged by Windows Audit Policies include logons, account management actions, directory service access, object access, policy changes, privilege use, process tracking, and system events. 

- In Group Policy Management Console, enable either basic audit policies under `Local Policies > Audit Policy` or advanced policies under `Security Settings > Advanced Audit Policy Configuration`. Using both may cause unexpected results in reporting (advanced policies may override all basic policies). 
- If using advanced policies, enable the `Audit: Force audit policy subcategory settings (Windows Vista or later) to override audit policy category settings` setting.
- Push Group Policy updates to all machines in the environment with `gpupdate /force`. Check the resulting policy with `auditpol /get /category:*`. 

- Ensure that audit policies at least meet Microsoft's baseline recommendations for event logging, found at <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations>. 
- Ensure audit policies are enabled on all machines.
- Check that all monitored events have all (or most) of the following attributes to minimize false positives:
	- High likelihood that occurrence means unauthorized or malicious activity.
	- Low number of false positives.
	- Event's occurrence results in an investigation. 
	- A single occurrence of the even indicates unauthorized activity OR an accumulation of events above an expected baseline. 
- Ensure that antivirus disabling or removal is logged.
- Ensure that privileged account actions are logged. 

### Monitor changes to audit policies

Adversaries may change policies to avoid detection or evade defenses.

- Audit and monitor events with the following IDs:
	- 4719 - System audit policy was changed 
	- 4817 - Auditing settings on an object were changed 
	- 4907 - Auditing settings on an object were changed 
	- 4912 - Per User Audit Policy was changed 
	- 4715 - The audit policy, SACL, on an object was changed

## Associated Techniques

- [T1484.001](https://attack.mitre.org/techniques/T1484/001) - Domain or Tenant Policy Modification: Group Policy Modification
- [T1222.001](https://attack.mitre.org/techniques/T1222/001) - File and Directory Permissions Modification: Windows File and Directory Permissions Modification
- [T1562.002](https://attack.mitre.org/techniques/T1562/002) - Impair Defenses: Disable Windows Event Logging
- [T1070.001](https://attack.mitre.org/techniques/T1070/001) - Indicator Removal: Clear Windows Event Logs

## Related Countermeasures

- CM0060 - Restrict and Monitor Domain Accounts with Local Administrator Access to Workstations
- CM0063 - Investigate Suspicious Login Attempts
- CM0064 - Investigate Account Manipulation
- CM0085 - Audit Group Policy Objects (GPOs) in Active Directory (AD)
- CM0044 - Monitor Account Creation and Permission Changes

## References

- Audit Policy Recommendations | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations>
- Audit Policy | <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/security-policy-settings/audit-policy>
- 4719(S): System audit policy was changed | <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4719>
- Advanced security auditing FAQ | <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/advanced-security-auditing-faq>