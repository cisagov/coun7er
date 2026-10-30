# Reset Directory Services Restore Mode (DSRM) Account Passwords on Domain Controllers

* **ID:** CM0069
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; DSRM accounts and domain controllers

## Intended Outcome

Resetting Directory Services Restore Mode (DSRM) account passwords on domain controllers terminates adversary persistence on local Active Directory (AD) databases via the DSRM account.

## Introduction

The DSRM account is a local administrator account in Windows Server operating systems used to restore or repair the server's Active Directory (AD) database should the database become corrupted inhibiting an administrator from logging in with their AD credentials. The DSRM account possesses full administrative rights, but not domain or network access rights. If an adversary has access the DSRM account, they will be able to achieve persistence by dumping the hash of the domain controller's password and modifying the registry key DsrmAdminLogonBehavior.

## Preparation

- Verify Windows server version supports `ntdsutil`. Older server versions (e.g., Windows 2000) may require the command `SETPWD`.
- Confirm HKLM\System\CurrentControlSet\Control\Lsa\DsrmAdminLogonBehavior is *not* set to 2. If DSRM is set to 2, the adversary likely modified its value to gain domain persistence. Administrators should change the registry key's default value to 0. 


## Risks

No Risks content identified. (Because the DSRM password is stored in the local Security Account Manager (SAM) database, other AD accounts should be unaffected.)

## Guidance

### Reset using Ntdsutil

System administrators can use the command prompt tool `ntdsutil` and the commands `set dsrm password` followed by `reset password on server` specifying either `null` or the server name to reset their DSRM passwords.

DSRM passwords are set on each individual domain controller and do not replicate across domain controllers in a domain. DSRM passwords should be unique from every other Domain Controller (DC) and changed regularly.

## Associated Techniques

- [T1003](https://attack.mitre.org/techniques/T1003) - OS Credential Dumping

## Related Countermeasures

- CM0050 - Reset Kerberos Ticket Granting Ticket (KRBTGT) Password Twice
- CM0073 - Rotate Active Directory Federation Services (AD FS) Certificates
- CM0077 - Revoke Microsoft 365 Refresh Tokens
- CM0033 - Reset User Account Passwords
- CM0098 - Enable Extended Protection for Authentication (EPA)
- CM0107 - Restore SYSVOL Content in AD Domains
- CM0122 - Restrict SYSVOL Credential Theft

## References

- How to reset the Directory Services Restore Mode administrator account password in Windows Server | <https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/reset-directory-services-restore-mode-admin-pwd>
- Detecting DSRM Account Misconfigurations | <https://www.sentinelone.com/blog/detecting-dsrm-account-misconfigurations/>
