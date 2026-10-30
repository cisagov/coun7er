# Reset Service Account Passwords

* **ID:** CM0028
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Identity administration; service accounts and password rotation

## Intended Outcome

Resetting service account passwords restricts adversary persistence and lateral movement using valid accounts.

## Introduction

There are different types of service accounts: built-in service accounts, traditional service accounts, and managed service accounts.
Please refer to the Microsoft Service Account Selection Matrix below which provides guidance for what and when a particular service account should be used. 

| Criterion | gMSA | sMSA | Computer Account | User account |
|-----------|------|------|------------------|--------------|
| App runs on a single server | Yes | Yes. Use a gMSA if possible | Yes. Use an MSA if possible | Yes. Use an MSA if possible |
| App runs on multiple servers | Yes | No | No. Account is tied to the server. | Yes. Use an MSA if possible. |
| App runs behind a load balancer | Yes | No | No | Yes. Use only if you can’t use a gMSA. |
| App runs on Windows Server 2008 R2 | No | Yes | Yes. Use an MSA if possible | Yes. Use an MSA if possible |
| App runs on Windows Server 2012 | Yes | Yes. Use a gMSA if possible | Yes. Use an MSA if possible. | Yes. Use an MSA if possible. |
| Requirement to restrict service account to single server | No | Yes | Yes. Use an sMSA if possible. | No |

In summary:

-   All Service Accounts: Any Windows Desktop Operating System (preferably attached to Active Directory)
-   Windows 2008 R2 and Above:  Virtual Service Account or Standalone Managed Service Account
-   Windows 2012 and Above: Group Managed Service Account

## Preparation

Documentation or knowledge of the purpose of the service accounts and potential impacts of a password reset is necessary to handle risks.  

## Risks

- This countermeasure can break legitimate functionality.

- Resetting Service Account passwords may cause crashes in ongoing processes that have dependencies on services that require authentication. For example, processes that depend on scheduled tasks will fail to execute due to password changes.

## Guidance

### Built-in Service Accounts

The accounts in this section do not have a password. Built-in service
accounts are: System Account, NetworkService Account, and
LocalService Account. These accounts do not appear in User
Management and cannot be added to groups in AD, but can be viewed in
Service Control Manager (SCM). Service Control Manager can be accessed
by pressing "Windows + R" to access the Run dialog box and entering
"services.msc".

### Traditional Service Accounts

A "traditional" service account is a standard user account configured to
run one or more services. Administrators and users may use their account
to run services because it is quicker and more convenient. However, this
will lead to issues when trying to track down which accounts are
associated with which services. Another issue that arises is creating a
new account for each service or a group of related services. Not only is
this a tedious task, but it is also problematic if you must manage the
passwords for all of these accounts. There's also the risk of breaking
applications or services associated with the changed passwords.
Therefore, organizations set these accounts to never expire and never
update them.

A solution to counter this would be to configure a group(s) in Active
Directory that contains accounts responsible for service(s). Proper
record keeping of what services these accounts are responsible for
should be kept so planned password resets can be accompanied with
credential updates for services. Also, temporary service disruption can
be planned and accounted for. Password resets can be done through Active
Directory or by using PowerShell cmdlets.

For consideration, do not add service accounts to privileged user
groups. This would enable services to run with elevated privileges and
give an attacker the ability to escalate privileges by compromising a
service or account. Each service should have its own account for
auditing and security purposes.

### Managed Service Accounts

Managed service accounts are designed for running services. Unique
passwords are automatically generated and changed every 30 days by
Active Directory. Interactive logon is not allowed; passwords are not
stored on the local system; and only Kerberos is used for
authentication. There are two types of managed service accounts,
standalone managed service accounts (sMSA) and group managed service
accounts.

Standalone managed service accounts (sMSAs) require windows server 2008
R2 and above. sMSAs can only run on one server; multiple services can be
run on that server. sMSAs cannot run scheduled tasks.

Group managed service accounts supersede sMSA and require Windows server
2012 or later. gMSAs can be used across multiple servers and can be used
to run scheduled tasks.

## Associated Techniques

- [T1110](https://attack.mitre.org/techniques/T1110/) - Brute Force
- [T1110.001](https://attack.mitre.org/techniques/T1110/001/) - Brute Force: Password Guessing
- [T1110.002](https://attack.mitre.org/techniques/T1110/002/) - Brute Force: Password Cracking
- [T1110.003](https://attack.mitre.org/techniques/T1110/003/) - Brute Force: Password Spraying
- [T1110.004](https://attack.mitre.org/techniques/T1110/004/) - Brute Force: Credential Stuffing
- [T1078.002](https://attack.mitre.org/techniques/T1078/002/) - Valid Accounts: Domain Accounts
- [T1078.003](https://attack.mitre.org/techniques/T1078/003/) - Valid Accounts: Local Accounts

## Related Countermeasures

- CM0050 - Reset Kerberos Ticket Granting Ticket (KRBTGT) Password Twice
- CM0070 - Reset Domain Controller (DC) Machine Account Password
- CM0079 - Reset Active Directory Services (AD DS) Connector Account Password
- CM0083 - Rotate Application Programming Interface (API) Keys
- CM0033 - Reset User Account Passwords
- CM0041 - Disable Service Account Interactive Login
- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts
- CM0044 - Monitor Account Creation and Permission Changes
- CM0092 - Reset Entra Seamless Sign-on Account Password
- CM0099 - Remove/Rotate Kubernetes Service Account Token
- CM0135 - Rebuild Group Managed Service Accounts (gMSA)

## References

- Windows LocalSystem vs. System | <https://serverfault.com/questions/168752/windows-localsystem-vs-system>
- Secure on-premises computer accounts with Active Directory | <https://learn.microsoft.com/en-us/entra/architecture/service-accounts-computer>
- Securing on-premises service accounts | <https://learn.microsoft.com/en-us/azure/active-directory/fundamentals/service-accounts-on-premises>
- LocalSystem Account | <https://learn.microsoft.com/en-us/windows/win32/services/localsystem-account>
- NetworkService Account | <https://learn.microsoft.com/en-us/windows/win32/services/networkservice-account>
- Local Accounts | <https://learn.microsoft.com/en-us/windows/security/identity-protection/access-control/local-accounts>
- LocalService Account | <https://learn.microsoft.com/en-us/windows/win32/services/localservice-account>
- Reset-ComputerMachinePassword | <https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/reset-computermachinepassword>
- Using Managed Service Accounts (MSA and gMSA) in Active Directory | <https://woshub.com/group-managed-service-accounts-in-windows-server-2012/>
- LocalService Account | <https://learn.microsoft.com/en-us/windows/win32/services/localservice-account>
