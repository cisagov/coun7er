# Audit and Restrict Exchange ApplicationImpersonation Role

* **ID:** CM0068
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine, Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Microsoft Exchange administration; RBAC and ApplicationImpersonation permissions

## Intended Outcome 

Auditing and restricting the Exchange ApplicationImpersonation role detects and disrupts adversary persistence and privilege escalation. 

## Introduction

The ApplicationImpersonation management role enables applications to impersonate users in an organization to perform tasks on behalf of the user. When an adversary adds this management role to an account, they can impersonate any user within the Exchange web services (EWS) environment. 

When adversaries assign this role, they do so typically to access the mailbox of users within a Microsoft365 tenant. While this role can be added using the Exchange Admin Center, it is more likely to be added via Exchange Online PowerShell (ExOPS). 

This role can both be created with limiting scopes to access the mailboxes of select users, or created without any explicit scopes, enabling an adversary to access all mailboxes in the tenant using impersonation. 

## Preparation

- Because adversaries require access to accounts with sufficient privileges to assign management roles, ensure passwords for all admin accounts are rotated, particularly Exchange Admins and Global Admins. 

## Risks

No Risks content identified.

## Guidance

If a security team suspects that an adversary may have abused the ApplicationImpersonation role, they should audit for commands that assign the ApplicationImpersonation role to any user accounts. 

If not already enabled, organizations should enable PowerShell logging following an incident to detect subsequent attempts by the adversary to add the ApplicationImpersonation role in addition to other malicious PowerShell operations.

Teams can verify whether ApplicationImpersonation was added to an account by exporting the management role configuration settings. This can be performed via ExOPS with the following command. 
`Get-ManagementRoleAssignment -Role ApplicationImpersonation`.

While auditing accounts that have the role, if an account with ApplicationImpersonation is identified that should not possess the role, administrators should remove the role where applicable. 
To remove unnecessary or overly permissive Management Role Assignments, use the Remove-ManagementRoleAssignment cmdlet.

If the account legitimately possesses the ApplicationImpersonation role, administrators should consider applying a scope to further limit the accounts the role can be used to impersonate and adding conditional access to restrict logons for accounts holding the role.

Note that because these impersonated events are not logged verbosely by the Unified Audit Logs, they can be difficult to detect. If it does not already exist, administrators should define rules to alert whenever the New-ManagementRoleAssignment operation is issued and filter for the assignment of the ApplicationImpersonation role.

## Associated Techniques

-	[T1098.002](https://attack.mitre.org/techniques/T1098/002/) - Account Manipulation: Additional Email Delegate Permissions
-	[T1548.005](https://attack.mitre.org/techniques/T1548/005/) - Abuse Elevation Control Mechanism: Temporary Elevated Cloud Access

## Related Countermeasures

- CM0074 - Audit and Restrict Mailbox Permissions
- CM0031 - Monitor PowerShell Program
- CM0033 - Reset User Account Passwords
- CM0044 - Monitor Account Creation and Permission Changes
- CM0109 - Audit and Restrict Cron Daemon

## References

- Impersonation and EWS in Exchange | <https://learn.microsoft.com/en-us/exchange/client-developer/exchange-web-services/impersonation-and-ews-in-exchange>
- Configure impersonation | <https://learn.microsoft.com/en-us/exchange/client-developer/exchange-web-services/how-to-configure-impersonation>
- Early Bird Catches the Wormhole: Observations from the StellarParticle Campaign | <https://www.crowdstrike.com/en-us/blog/observations-from-the-stellarparticle-campaign/>
- Remediation and Hardening Strategies for Microsoft 365 to Defend Against APT29 (v1.3) | <https://www.mandiant.com/sites/default/files/2022-08/remediation-hardening-strategies-for-m365-defend-against-apt29-white-paper.pdf>
