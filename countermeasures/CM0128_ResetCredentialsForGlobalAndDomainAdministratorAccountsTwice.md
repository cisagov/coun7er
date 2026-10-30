# Reset Credentials for Global and Domain Administrator Accounts Twice

* **ID:** CM0128
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory/Entra ID administration; privileged accounts and repeated credential rotation

## Intended Outcome

Resetting credentials for global and domain administrators twice prevents an adversary's persistence using compromised global or domain administrator accounts. 

## Introduction

Global and domain administrators have elevated privileges in a business environment, making them valuable targets for attackers. Changing credentials of privileged accounts can remove adversary footholds in the network, but some operating systems store hashes of both the current and previous passwords. To flush all compromised credentials, two resets are required. 

Adversaries may use pass-the-hash or ticket attacks to grab hashes and continue authenticating even through a password reset.  

## Preparation

- Identify all global and domain administrator accounts. Microsoft recommends not having more than 5 global administrators; this list should not be long. 
- Identify the organization's password policy.

## Risks

- Only Global Administrators or Privileged Authentication Administrators can change passwords for Global Administrators. Ensure that the account used for password resets has at least those permissions. 
- Lockouts from administrative accounts is possible if the organization's password reset policies are not followed or in cases that proper documentation of password resets is not conducted. 

## Guidance

The password does not need time to replicate through the network and resets can be done right after the other. 

- Reset the password manually for all global and domain administrator accounts. 
- Via the Microsoft Entra administrator center, navigate to `Identity > Users > All users` and select the account of the global or domain administrator.
- Select `Reset Password`. 

## Associated Techniques

- [T1078](https://attack.mitre.org/techniques/T1078) - Valid Accounts

## Related Countermeasures

- CM0061 - Rotate Mailbox Passwords
- CM0063 - Investigate Suspicious Login Attempts
- CM0071 - Unenroll Suspicious Multifactor Authentication (MFA) Device from Microsoft User Account 
- CM0084 - Restrict Accounts with Privileged Active Directory (AD) Access from Logging into Endpoints
- CM0126 - Reset Credentials for Local Administrator Accounts Twice
- CM0145 - Reset Interdomain Trust Account (ITA) Credentials

## References

- Global Administrator | <https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/permissions-reference#global-administrator>
- Privileged roles and permissions in Microsoft Entra ID (preview) | <https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/privileged-roles-permissions>