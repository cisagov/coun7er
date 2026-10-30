# Deny Logon as a Batch Job

* **ID:** CM0103
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; user rights and batch logon restrictions

## Intended Outcome

Denying log on as a batch job for a particular account disrupts the ability for an adversary to automate tasks or execute malicious scripts at specified times. 

## Introduction

No Introduction content identified.

## Preparation

- Before assigning an account the "Deny log on as a batch job" user right, assess the impact of the countermeasure on the job activities of the affected accounts. If containing the adversary, consider first inhibiting their access to the compromised account by rotating the credentials of the compromised account. 
- If "log on as a batch job" is required by a service account, create a Group Managed Service Account (gMSA) and delegate only the necessary privileges for the service running the task(s). 

## Risks

- Removing the ability for a user or group to log on as a batch job may disrupt legitimate processes or services that rely on privilege. By default, logon as a batch job is enabled for administrators and backup operators on domain controllers and stand-alone servers so denying this ability may introduce downtime to domain controllers and administrative servers.

## Guidance

Ensure that the Guest user group is denied the ability to log on as a batch job. Extend "deny log on as a batch job" to Local Accounts, Domain Admins, Enterprise Admins, and all other user accounts as necessary. 

Confirm Domain Admins and Enterprise Admins in an AD Domain are denied logons on lower trust systems and ensure auditing is configured to alert if modifications are made to properties or memberships of highly-privileged groups. 

### Remove the ability to log on as a batch job using Group Policy Management Console (GPMC)

After editing an existing Group Policy Object (GPO) or creating a new GPO, navigate to
Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > User Rights Assignment
from the Group Policy Editor.

From here, the properties menu can be opened for "Deny log on as a batch job" and a user account or group added. 

After updating "Deny log on as a batch job," open the properties window of "Log on as a batch job" and remove any unauthorized Users or Groups that possess the right to log on as a batch job. While "Deny log on as a batch job" overrides "Log on as a batch job," users should still be removed from the latter if a user has both.

After completing these steps, link the GPO to the appropriate domain, site, or organizational unit (OU) to apply the policy. 

### Remove the ability of a user to log on as a batch job using PowerShell

In addition to the above method, an administrator may use PowerShell to create a GPO and deny log on as a batch job.

## Associated Techniques

- [T1053.005](https://attack.mitre.org/techniques/T1053/005/) - Scheduled Task/Job: Scheduled Task
- [T1053](https://attack.mitre.org/techniques/T1053/) - Scheduled Task/Job

## Related Countermeasures

- CM0060 - Restrict and Monitor Domain Accounts with Local Administrator Access to Workstations
- CM0038 - Remove Windows Management Instrumentation (WMI) Event Subscription
- CM0109 - Audit and Restrict Cron Daemon
- CM0120 - Remove Cron Job
	

## References

- Deny log on as a batch job | <https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/deny-log-on-as-a-batch-job>
- Log on as a batch job | <https://www.ultimatewindowssecurity.com/wiki/page.aspx?spid=LogonAsBatch>
- The "Deny log on as a batch job" user right will be configured to include "Guests". | <https://www.stigviewer.com/stig/windows_7/2012-07-02/finding/V-26483>
- Windows Server 2019 Deny log on as a batch job user right on domain-joined member servers must be configured to prevent access from highly privileged domain accounts and from unauthenticated access on all systems. | <https://www.stigviewer.com/stig/windows_server_2019/2019-07-09/finding/V-93011>
- The Deny log on as a batch job user right on domain controllers must be configured to prevent unauthenticated access. | <https://www.stigviewer.com/stig/windows_server_2016/2018-09-05/finding/V-73761>
- Appendix E: Securing Enterprise Admins Groups in Active Directory | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/appendix-e--securing-enterprise-admins-groups-in-active-directory>
