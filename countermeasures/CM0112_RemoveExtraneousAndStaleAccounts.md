# Remove Extraneous and Stale Accounts

* **ID:** CM0112
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Eliminate
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Identity administration; account lifecycle management and stale account removal

## Intended Outcome

Removing extraneous and stale accounts blocks or restricts adversary initial access, persistence, defense evasion, and lateral movement using said accounts. 

## Introduction

Extraneous or stale accounts are accounts that are considered no longer active in an environment and may be unmonitored or have expired credentials, making them targets for attackers. They can also be used by former employees whose accounts are still active, posing additional security risks. 

## Preparation

- Identify organization's standard procedures regarding inactive user accounts and how long after last logon or password reset an account is considered "inactive".
- Identify de-provisioning procedures for user accounts. 

## Risks

No Risks content identified. 

## Guidance 

This countermeasure addresses only extraneous user accounts for employees and other contractors; auditing and removing service accounts for legacy applications may require more nuance than is presented here. 

- Audit all Active Directory (AD) users where the password age is over the company's policy for inactivity. The following PowerShell command locates all users where password hasn't changed for 180 days (6 months): `Get-ADUser -Filter '(PasswordLastSet -lt $d) -or (LastLogonTimestamp -lt $d)' -Properties PasswordLastSet,LastLogonTimestamp | ft Name,PasswordLastSet,@{N="LastLogonTimestamp";E={[datetime]::FromFileTime($_.LastLogonTimestamp)}}`, where `$d = [DateTime]::Today.AddDays(-180)`.
- Disable all found accounts and move them to a separate organizational unit (OU).
- After de-provisioning, certain circumstances may warrant deleting the inactive account. While deleting accounts reduces the attack surface, it may lead to complications with services linked to the AD or cause issues should the account be needed again in the future. Defer to the organization's standard account de-provisioning procedures.  

## Associated Techniques

- [T1078](https://attack.mitre.org/techniques/T1078) - Valid Accounts 

## Related Countermeasures

- CM0063 - Investigate Suspicious Login Attempts
- CM0064 - Investigate Account Manipulation
- CM0071 - Unenroll Suspicious Multifactor Authentication (MFA) Device from Microsoft User Account
- CM0033 - Reset User Account Passwords
- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts
- CM0044 - Monitor Account Creation and Permission Changes
- CM0118 - Revoke Existing Administrator Permissions and Deploy New Administrator Accounts

## References

- Regularly check for and remove inactive user accounts in the Active Directory | <https://learn.microsoft.com/en-us/services-hub/unified/health/remediation-steps-ad/regularly-check-for-and-remove-inactive-user-accounts-in-active-directory>

