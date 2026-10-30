# Reset Kerberos Ticket Granting Ticket (KRBTGT) Password Twice

* **ID:** CM0050
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; Kerberos and KRBTGT account recovery

## Intended Outcome

Resetting the Kerberos Ticket Granting Ticket (KRBTGT) password twice will terminate adversary credential access via Golden Ticket attacks.

## Introduction

The KRBTGT account is the Kerberos Key Distribution Center (KDC) service account and is responsible for encrypting and signing all Kerberos ticket requests. The KRBTGT account's password should be reset twice as the account has a two-password history. Failing to reset KRBTGT twice risks another Domain Controller using an old password. 

## Preparation

- Verify AD Replication health status.
- Consult Microsoft Active Directory Forest Recovery - Reset the krbtgt password. Available at <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/forest-recovery-guide/ad-forest-recovery-reset-the-krbtgt-password>

## Risks

- Resetting the KRBTGT account password will invalidate all AD dependent applications tickets. Non-windows clients may be unable to request new tickets until existing tickets expire. 
- If the KRBTGT account password has never been rotated before, domains may experience replication issues due to KRBTGT account credentials utilizing outdated ciphers (such as DES or RC4). 

## Guidance

The first account reset for the KRBTGT needs to be allowed to replicate prior to the second reset to avoid any issues. Waiting at least 10 hours before performing a second reset will ensure the reset fully propagates within large AD environments. If the maximum lifetime for a user ticket or service ticket policy settings have been modified, the amount of time to wait should be more than the configured value. 

### Reset KRBTGT Password with PowerShell

The KRBTGT account password can be reset twice by using Microsoft's New-KrbtgtKeys script 

The script is available at <https://github.com/microsoftarchive/New-KrbtgtKeys.ps1>.

Organizations using Read-Write Domain Controllers will need to run the script separately; once for Read-Write Domain Controllers and once for Read-Only Domain Controllers.


### Reset KRBTGT Password with Administrative Tools

The KRBTGT account password can also be reset by using the Control Panel with Administrative Tools and resetting the password for the KRBTGT User account under Active Directory Users and Computers.

## Associated Techniques

- [T1558](https://attack.mitre.org/techniques/T1558/) - Steal or Forge Kerberos Tickets 
- [T1558.001](https://attack.mitre.org/techniques/T1558/001/) - Steal or Forge Kerberos Tickets: Golden Ticket 



## Related Countermeasures

- CM0028 - Reset Service Account Passwords
- CM0069 - Reset Directory Services Restore Mode (DSRM) Account Passwords on Domain Controllers
- CM0070 - Reset Domain Controller (DC) Machine Account Password
- CM0073 - Rotate Active Directory Federation Services (AD FS) Certificates
- CM0077 - Revoke Microsoft 365 Refresh Tokens
- CM0083 - Rotate Application Programming Interface (API) Keys
- CM0092 - Reset Entra Seamless Sign-on Account Password
- CM0126 - Reset Credentials for Local Administrator Accounts Twice
- CM0135 - Rebuild Group Managed Service Accounts (gMSA)
- CM0145 - Reset Interdomain Trust Account (ITA) Credentials

## References

- Active Directory Forest Recovery - Reset the krbtgt password | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/forest-recovery-guide/ad-forest-recovery-reset-the-krbtgt-password>
- FAQs from the Field on KRBTGT Reset | <https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/faqs-from-the-field-on-krbtgt-reset/ba-p/2367838>
- Practical Compromise Recovery Guidance for Active Directory | <https://m365internals.com/2021/04/27/practical-compromise-recovery-guidance-for-active-directory/>
- Eviction Guidance for Networks Affected by the SolarWinds and Active Directory/M365 Compromise | <https://www.cisa.gov/news-events/analysis-reports/ar21-134a>
