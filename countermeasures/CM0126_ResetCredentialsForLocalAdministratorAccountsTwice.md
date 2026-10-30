# Reset Credentials for Local Administrator Accounts Twice

* **ID:** CM0126
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; local administrator accounts and repeated credential rotation

## Intended Outcome

Resetting credentials for local administrator accounts twice blocks or prevents an adversary's initial access and persistence via local administrator accounts. 

## Introduction

Changing credentials of privileged accounts can remove adversary footholds in the network, but some operating systems store hashes of both the current and previous passwords. To flush all compromised credentials, two resets are required. 

Adversaries may use pass-the-hash or ticket attacks to grab hashes and continue authenticating even through a password reset.  

## Preparation

- Identify all local administrator accounts. 
- Identify the organization's password policy. 

## Risks

- Lockout of local administrative accounts is likely if proper documentation of password resets is not conducted or policy is not followed. 

## Guidance

The password does not need time to replicate through the network and resets can be done right after the other. 

- Reset the password manually for all local administrator accounts. This can be done via PowerShell:
	- The `net user <username> *` command prompts a reset for the provided account. The command must be run on the machine that has the account.
	- If Local Administrator Password Solution (LAPS) is enabled, the `Reset-LapsPassword []` cmdlet will immediately reset the password of the account.  
- Via the Microsoft Entra administrator center:
	- Navigate to `Identity > Users > All users` and select the account of the local administrator.
	- Select `Reset Password`. 


## Associated Techniques

- [T1078](https://attack.mitre.org/techniques/T1078) - Valid Accounts

## Related Countermeasures

- CM0050 - Reset Kerberos Ticket Granting Ticket (KRBTGT) Password Twice
- CM0060 - Restrict and Monitor Domain Accounts with Local Administrator Access to Workstations
- CM0061 - Rotate Mailbox Passwords
- CM0063 - Investigate Suspicious Login Attempts
- CM0071 - Unenroll Suspicious Multifactor Authentication (MFA) Device from Microsoft User Account 
- CM0084 - Restrict Accounts with Privileged Active Directory (AD) Access from Logging into Endpoints
- CM0128 - Reset Credentials for Global and Domain Administrator Accounts Twice
- CM0145 - Reset Interdomain Trust Account (ITA) Credentials



## References

- Reset a user's password | <https://learn.microsoft.com/en-us/entra/fundamentals/users-reset-password-azure-portal>
- Reset-LapsPassword | <https://learn.microsoft.com/en-us/powershell/module/laps/reset-lapspassword?view=windowsserver2022-ps>
- Get started with Windows LAPS and Microsoft Entra ID | <https://learn.microsoft.com/en-us/windows-server/identity/laps/laps-scenarios-azure-active-directory#update-a-password-in-microsoft-entra-id>
