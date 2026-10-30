# Restrict Accounts with Privileged Active Directory (AD) Access from Logging into Endpoints

* **ID:** CM0084
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; privileged accounts and endpoint logon rights

## Intended Outcome

Restricting accounts with privileged Active Directory (AD) access from logging into endpoints blocks adversary lateral movement via privileged AD access.

## Introduction

Accounts with domain privileges should only be accessed through designated Privileged Access Workstations (PAWs). If an adversary compromises a privileged or service account, they may attempt to perform lateral movement by accessing workstations, common access servers, and other endpoints. To disrupt the ability of an adversary to access these endpoints, system administrators should block accounts with privileged AD access, such as Domain Admins, from being able to log into unauthorized endpoints.

## Preparation

- Assess the impact of the mitigation on the job activities of the affected accounts. 
- If containing the adversary, inhibiting their access to the compromised account first by first rotating credentials.   
- Verify whether the user right is absolutely required by a service account. If so, create a Group Managed Service Account (gMSA) and delegate only the necessary privileges for the service running the task(s). 

## Risks

- Removing the ability for a service account to log into an endpoint may disrupt services or processes on the endpoint.

## Guidance

### Apply Deny Log On User Rights

To deny accounts with privileged AD access from logging into endpoints, the following restrictions should be enforced on the account(s):

- Deny access to this computer from the network (SeDenyNetworkLogonRight)
- Deny logon as a service (SeDenyServiceLogonRight)
- Deny log on as a batch job (SeDenyBatchLogonRight)
- Deny log on locally (SeDenyInteractiveLogonRight)
- Deny log on through Remote Desktop (sometimes known as Deny log on through Terminal Services) (SeDenyRemoteInteractiveLogonRight)

These user right restrictions may be assigned to a new or existing Group Policy Object (GPO) using the Group Policy Editor (GPE) (gpedit.msc)

Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > User Rights Assignment 

When assigning the "deny access to this computer from the network", include the security identifier (SID) flag S-1-5-114.

Afterwards, link the GPO to the appropriate domain, site, or organizational unit (OU).

In addition to the above method, an administrator may use PowerShell to configure user rights. 

### Remove Unauthorized Log On User Rights

After applying the 'deny' user rights to an account or group, system administrators should remove the equivalent 'log on' user rights. These include

- Access this computer from the network (SeNetworkLogonRight)
- Logon as a service (SeServiceLogonRight)
- Log on as a batch job (SeBatchLogonRight)
- Allow log on locally (SeInteractiveLogonRight)
- Allow log on through Remote Desktop Services (SeRemoteInteractiveLogonRight)

While the "deny" rights override the "log on" rights, accounts should be configured to possess either one or the other - not both. 

Force Group Policies updates with `Gpupdate /force` to ensure the policy changes are applied.

## Associated Techniques

- [T1078](https://attack.mitre.org/techniques/T1078/) - Valid Accounts
- [T1078.003](https://attack.mitre.org/techniques/T1078/003/) - Valid Accounts: Local Accounts
- [T1078.002](https://attack.mitre.org/techniques/T1078/002/) - Valid Accounts: Domain Accounts

## Related Countermeasures

- CM0059 - Configure Tactical Privileged Access Workstation
- CM0060 - Restrict and Monitor Domain Accounts with Local Administrator Access to Workstations
- CM0063 - Investigate Suspicious Login Attempts
- CM0041 - Disable Service Account Interactive Login
- CM0111 - Enable Multiple Administrative Approval (MAA) for Access Policies
- CM0114 - Enable Microsoft Conditional Access Policy Templates
- CM0124 - Disable On-Premise Active Directory (AD) Accounts with Privileged Roles in Entra ID
- CM0126 - Reset Credentials for Local Administrator Accounts Twice
- CM0128 - Reset Credentials for Global and Domain Administrator Accounts Twice

## References

- Proactive Preparation and Hardening to Protect Against Destructive Attacks | <https://cloud.google.com/blog/topics/threat-intelligence/protect-against-destructive-attacks/>
- Implementing Least-Privilege Administrative Models | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models>
