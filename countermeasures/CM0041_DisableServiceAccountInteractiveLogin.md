# Disable Service Account Interactive Login

* **ID:** CM0041
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Identity administration; service accounts and logon restrictions


## Intended Outcome

Disabling service account interactive login restricts adversary lateral movement using service accounts.

## Introduction

An interactive logon is a software method in which the account information and credentials input by the user interactively are authenticated by a server or domain controller.

## Preparation

- Disable compromised user account.
- Disconnect the system(s) from the network.

## Risks

- Errors can render this countermeasure ineffective.
- If an adversary has compromised a user account with existing administrative rights, they will maintain the ability to install and configure services to run using the System account. Evict the adversary from retaining any control over privileged accounts or access to the network before assigning this right.
- This countermeasure may not stop all ongoing activity.
- Adversaries can still bypass AD group policies by brute-force or if default passwords are used.

## Guidance

### Disabling Interactive Logins for Service Accounts Manually

To manually assign this policy to a user and/or return the policy to its original state, use the Group Policy Object Editor or the Local Security Policy console. When modifying user rights associated with service accounts, it is recommended to assign the right via a Group Policy Object (GPO) as opposed to locally.

### Disabling Interactive Logins for Service Accounts via GPO

To deny interactive logons for service accounts, system administrators should select the appropriate organizational unit (OU) via the Group Policy Management Console and either edit an existing group policy object (GPO) or create a new one. (There may be an existing GPO serving as a special security group with the excepted services that need to be modified.) 

- In the Group Policy Management Editor, administrators should navigate to *Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > User Rights Assignment*
- Administrators should add the appropriate user accounts to both the "Deny log on locally" policy and the "Deny log on through Terminal Services."
- After linking these policies to the OU, the policy will apply after rebooting the machines. Both policies take priority over the "Log on locally" and "Log on Through Terminal Services" rights.

## Associated Techniques

- [T1078.002](https://attack.mitre.org/techniques/T1078/002) - Valid Accounts: Domain Accounts
- [T1078.003](https://attack.mitre.org/techniques/T1078/003) - Valid Accounts: Local Accounts

## Related Countermeasures

- CM0028 - Reset Service Account Passwords
- CM0084 - Restrict Accounts with Privileged Active Directory (AD) Access from Logging into Endpoints
- CM0099 - Remove/Rotate Kubernetes Service Account Token

## References

- Red Flag Alert: Service Accounts Performing Interactive Logins | <https://www.crowdstrike.com/en-us/blog/service-accounts-performing-interactive-logins/>
- Glossary - interactive logon | <https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-authsod/64781df1-ee20-413e-b8c5-6511c90dbc30#gt_9360639b-135c-46dc-9f9e-85728008146f>
