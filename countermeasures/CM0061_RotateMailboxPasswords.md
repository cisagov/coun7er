# Rotate Mailbox Passwords

* **ID:** CM0061
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Email/identity administration; mailbox accounts and password rotation

## Intended Outcome 

Rotating mailbox passwords reduces the risk of unauthorized access, data breaches, and mailbox misuse for malicious purposes.

## Introduction

Adversaries that target an email account may intend to collect information, weaponize the account for spear phishing, or repurpose the account for data exfiltration. Rotating mailbox credentials will disrupt the adversary's access to the email account. 

## Preparation

- Organizations using Microsoft365 should consult Microsoft's instructions for responding to a compromised email account and implement multi-factor authentication as part of their remediation strategy.
- Organizations can prepare in advance by developing a standard operating procedure when pushing out mandatory password resets to affected accounts. These instructions should include guidance for alerting users and a means for users to confirm the veracity of password reset requests to reduce the risk for future social engineering attacks.
- Know or find out who your mailbox users are in your environment.

## Risks

- User account password reset events could potentially alert an adversary to defensive measures being taken against their actions. 
- Users may forget newly created passwords and result in temporary account lockouts.
- Administrators may reuse passwords and result in adversaries retaining or gaining access.

## Guidance

### Rotating Passwords

Organizations should consult the documentation provided by their respective email and personal information manager provider and follow the vendor's instructions for performing password resets. 

For example, if the organization is subscribed to Microsoft365, administrators will be able to reset an individual's password (and therefore enterprise mailbox password) via the Microsoft 365 admin center. Alternately, administrators may instead use the PowerShell cmdlet Set-ADAccountPassword to perform the same action. 

## Associated Techniques

- [T1110](https://attack.mitre.org/techniques/T1110/) - Brute Force
- [T1555](https://attack.mitre.org/techniques/T1555) - Credentials from Password Stores
- [T1114.003](https://attack.mitre.org/techniques/T1114/003/) - Email Collection: Email Forwarding Rule

## Related Countermeasures

- CM0074 - Audit and Restrict Mailbox Permissions
- CM0082 - Identify and Remove Suspicious Email Forwarding Rules
- CM0126 - Reset Credentials for Local Administrator Accounts Twice
- CM0128 - Reset Credentials for Global and Domain Administrator Accounts Twice

## References

- Responding to a compromised email account | <https://learn.microsoft.com/en-us/microsoft-365/security/office-365-security/responding-to-a-compromised-email-account?view=o365-worldwide>
- Reset passwords for Microsoft 365 for business | <https://learn.microsoft.com/en-us/microsoft-365/admin/add-users/reset-passwords?view=o365-worldwide&redirectSourcePath=%252farticle%252fadmins-reset-office-365-business-passwords-7a5d073b-7fae-4aa5-8f96-9ecd041aba9c>
