# Revoke Existing Administrator Permissions and Deploy New Administrator Accounts

* **ID:** CM0118
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Identity administration; privileged account replacement and permission revocation, Microsoft Entra

## Intended Outcome

Revoking existing administrator accounts and permissions and deploying new ones blocks an adversary from persistence, lateral movement, and privilege escalation using existing administrator credentials. 

## Introduction

Administrator and other privileged accounts are high priority targets for adversaries for lateral movement and privilege escalation. To evict adversaries out of an environment where they hold privileged access, all privileged accounts need completely new credentials, tokens, and access control. 

## Preparation

- Identify all administrator and privileged accounts in the environment.
- Determine what time interval for changing credentials would cause the least disruption to business operations. 
- Locate the administrator account provisioning policy, if one exists for the organization.  

## Risks

- Mass credential resets are disruptive to business operations. Administrators will lose access to their accounts and files for some time before new accounts are set up and configured. Consider deprovisioning and re-provisioning accounts during times during slower times of the workday.
- Improper configuration or provisioning of accounts may cause all administrators to be locked out of the system and unable to revert changes. Peer review all configurations before rolling out changes. 


## Guidance

Credential revocation and assignment can be done through Group Policy Objects (GPOs) for all users or machines in organizational units, or permissions can be revoked manually on each machine. In Active Directory (AD) environments, managing permissions through GPOs is recommended. 

### Revoke existing permissions

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

### Create and deploy new accounts

To fully remove an adversary's foothold in a network, it may be necessary to create new accounts for all users whose administrative privileges got revoked in the previous step, so that their accounts are no longer under adversary control but they have all the roles necessary to do their jobs. To create new users, the User Administrator role is required.
- In Microsoft Entra admin center, navigate to `Identity > Users > All users`.
- Select `New user > Create new user`.
- Create the new user by filling out appropriate remaining fields. Repeat as necessary.
- Assign necessary roles to the user. 

## Associated Techniques

- [T1078](https://attack.mitre.org/techniques/T1078) - Valid Accounts

## Related Countermeasures

- CM0063 - Investigate Suspicious Login Attempts
- CM0064 - Investigate Account Manipulation
- CM0071 - Unenroll Suspicious Multifactor Authentication (MFA) Device from Microsoft User Account
- CM0033 - Reset User Account Passwords
- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts
- CM0044 - Monitor Account Creation and Permission Changes
- CM0111 - Enable Multiple Administrative Approval (MAA) for Access Policies
- CM0112 - Remove Extraneous and Stale Accounts

## References

- Effective strategies for conducting Mass Password Resets during cybersecurity incidents | <https://techcommunity.microsoft.com/t5/microsoft-security-experts-blog/effective-strategies-for-conducting-mass-password-resets-during/ba-p/4159408>
- How to create, invite, and delete users | <https://learn.microsoft.com/en-us/entra/fundamentals/how-to-create-delete-users>
- Adding Domain Users to the Local Administrators Group in Windows | <https://woshub.com/add-domain-users-local-admin-group-gpo/>