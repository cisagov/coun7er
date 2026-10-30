# Disable On-Premise Active Directory (AD) Accounts with Privileged Roles in Entra ID

* **ID:** CM0124
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory/Entra ID administration; hybrid identities and privileged account restrictions

## Intended Outcome

Disabling privileged on-premises Active Directory (AD) accounts in Entra ID prevents an adversary's attempts to achieve persistence or privilege escalation by abusing administrator or domain accounts.

## Introduction

An on-premises domain is managed by the organization, as opposed to a managed domain via Microsoft Entra Domain Services, which is managed by Microsoft. On-premises domains are often integrated with Entra ID for reliability, security, and cost optimization. Due to integration, users on-premises use their same credentials to authenticate with Entra ID and the managed domain. 

Adversaries may use compromised on-premises administrator accounts to authenticate with Entra ID and exploit resources available in the managed domain.  

## Preparation

- Identify all on-premises AD accounts with privileged roles in Entra ID to be disabled.
- Identify organization's standard account de-provisioning procedure for administrator and other privileged accounts. 
- Identify any emergency access accounts.

## Risks

- Administrators in the on-premises domain will not be able to access their accounts. In the case that all administrators are locked out, consider using emergency access accounts to restore access. The default *support* and *cyberx* accounts may be used.
- Accounts removed from on-premises domains do not automatically get removed from Entra ID. If an account needs to be removed from both locations, remove it from Entra ID as well. 

## Guidance

Users can be removed or disabled manually through PowerShell or removed from the administrator groups using Group Policy Objects (GPOs). This section provides guidance on disabling administrator accounts in both an on-premises domain and in Entra ID. 

To find all users with an administrator (or other privileged) role, navigate to `Identity > Roles & admins > Roles & admins` in the Microsoft Entra admin center and select the desired role. The `Assignments` page will list all the users with that role. 

### Manually disable on-premises users 

These commands require administrator permissions. 

- Run `Disable-ADAccount -Identity <account_to_disable>`.
- Reset that user's password twice in the AD. 

### Manually disable Entra ID users

Entra ID provides steps both to remove roles from a user account to revoke their administrator privileges, or to delete the account entirely. Defer to the organization's account de-provisioning procedures. 

- To remove an administrator role:
	- Open Microsoft Entra admin center, navigate to `Identity > Users > All users`.
	- Select the user to remove the role from and select `Assigned Roles`.
	- Select the role to remove and click `Remove assignment`.
	
- To delete a user account:
	- Open Microsoft Entra admin center, navigate to `Identity > Users > All users`.
	- Select the user to delete, select `Delete`. 

### Remove users from privileged accounts using Group Policy

Locate the administrator account deprovisioning policy. If one does not exist, create a new GPO policy, then edit it:
- In the policy, navigate to `Computer Configuration > Preferences > Control Panel Settings > Local Users and Groups`.
- Add a new rule for the local group.
- Select the `Update` option in the Action field.
- Select the privilege level from the Group Name dropdown.
- Add all administrators to the group.
- Select the `Remove from this group` action. 
- Link the new policy with the Computer organizational unit (OU).
- If the account was only used for privileged access and can safely be removed from the environment once disabled, delete the account.  
- Force the GPO update using the `gpupdate /force` command. 

## Associated Techniques

- [T1078](https://attack.mitre.org/techniques/T1078) - Valid Accounts
- [T1098](https://attack.mitre.org/techniques/T1098) - Account Manipulation

## Related Countermeasures

- CM0060 - Restrict and Monitor Domain Accounts with Local Administrator Access to Workstations
- CM0063 - Investigate Suspicious Login Attempts
- CM0071 - Unenroll Suspicious Multifactor Authentication (MFA) Device from Microsoft User Account 
- CM0084 - Restrict Accounts with Privileged Active Directory (AD) Access from Logging into Endpoints 
- CM0033 - Reset User Account Passwords
- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts

## References

- Manage emergency access accounts in Microsoft Entra ID | <https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/security-emergency-access>
- What is Microsoft Entra Domain Services? | <https://learn.microsoft.com/en-us/entra/identity/domain-services/overview>
- Revoke user access in Microsoft Entra ID | <https://learn.microsoft.com/en-us/entra/identity/users/users-revoke-access#revoke-access-for-a-user-in-the-hybrid-environment>
- Add and manage admin accounts | <https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-manage-admin-accounts>
