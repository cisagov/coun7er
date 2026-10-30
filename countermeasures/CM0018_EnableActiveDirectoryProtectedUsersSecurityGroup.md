# Enable Active Directory (AD) Protected Users Security Group

* **ID:** CM0018
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; privileged groups and Protected Users

## Intended Outcome

Enabling the Active Directory (AD) Protected Users security group
restricts adversary lateral movement using protected user credentials.

## Introduction

The Protected Users security group is designed as part of a strategy to
manage credential exposure within the enterprise. Members of this group
automatically have non-configurable protections applied to their
accounts. Membership in the Protected Users group is meant to be
restrictive and proactively secure by default. The only method to modify
these protections for an account is to remove the account from the
security group.

## Preparation

- Confirm that primary domain controller is running at least Windows Server 2012 R2.
- Confirm that client computers are running at least Windows 8.1 or Windows Server 2012 R2.
- If systems to not meet above criteria, protected user group will not be available.

## Risks

- This countermeasure can break legitimate functionality.
- Adding Managed Service Accounts to "Protected Users" will break their functionality, by breaking Kerberos Constrained Delegation (KCD).
- Responders are advised against this countermeasure for this reason.

## Guidance

- Add “Admin” to the Protected Users Group
- Verify Protected User is Added
- Best Practices
    - The Protected Users Group protections may be replicated to OSes below Windows Server 2012 R2 if a Windows Server 2012 R2 or above is the Primary Domain Controller emulator.
    - The Protected Users Group protections will not work for client devices that are NOT Windows 8.1.
    - Maintain situational awareness on authentication attempts by reviewing “Protected Users” user and failure logs.
    - Never add all privileged accounts to the Protected Users Group since there is potential to be locked out.

## Associated Techniques

No Associated Techniques content identified.

## Related Countermeasures

- CM0063 - Investigate Suspicious Login Attempts
- CM0029 - Reset NT Hashes for Smart Card-enabled Accounts
- CM0071 - Unenroll Suspicious Multifactor Authentication (MFA) Device from Microsoft User Account
- CM0072 - Disable Credential Caching
- CM0033 - Reset User Account Passwords
- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts
- CM0089 - Enable Local Security Authority (LSA) Protections
- CM0090 - Detect Attempts to Access Local Security Authority Subsystem Service (LSASS) Process
- CM0096 - Prevent and Detect Attempts to Copy NT Directory Services Database Information Table (NTDS.dit)
- CM0107 - Restore SYSVOL Content in AD Domains
- CM0104 - Enable Interactive Logon and Require Smart Card Policy for Privileged User Accounts
- CM0121 - Enable Managed Domain Authentication
- CM0122 - Restrict SYSVOL Credential Theft
- CM0125 - Reset and Monitor adminCount Attribute

## References

- Guidance about how to configure protected accounts | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/how-to-configure-protected-accounts>
- Ten things you need to be aware of before using the Protected Users Group | <https://dirteam.com/sander/2014/11/25/ten-things-you-need-to-be-aware-of-before-using-the-protected-users-group/>
- Step-by-Step Guide to Active Directory "Protected Users Security Group” | <https://www.rebeladmin.com/step-step-guide-active-directory-protected-users-security-group/>
- Protected Users Security Group | <https://learn.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/protected-users-security-group>
