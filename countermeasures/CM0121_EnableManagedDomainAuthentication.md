# Enable Managed Domain Authentication

* **ID:** CM0121
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Microsoft Entra Domain Services administration; managed domains and authentication

## Intended Outcome

Enabling managed domain authentication blocks or prevents abuse of legacy authentication methods or misconfigured domain infrastructure to achieve initial access or persistence. 

## Introduction

Microsoft provides managed domains via Entra Domain Services, providing services such as Lightweight Directory Access Protocol (LDAP) and Kerberos authentication and manages domain controllers in the cloud. This takes strain off of an organizations system administrators because Microsoft takes over managing and creating required resources. It also allows organizations to move legacy applications to a cloud domain from an on-premises active directory (AD) without having to manage their own cloud-based domain controllers. Domain services integrates with an existing Microsoft Entra tenant (previously Azure AD) and allows users to sign into applications and services with those credentials; authentication is handled via Azure AD, or Entra. 

Domain Services does not store credentials in plaintext or hashes in a format compatible with authentication protocols like Kerberos or NT LAN Manager (NTLM) by default. For users to authenticate using their existing credentials, Domain Services need to be enabled with the tenant.

Attackers can abuse legacy applications that don't support modern authentication methods to gain access to resources or user accounts. Self-managed domains may also be misconfigured or resources may not be properly managed, allowing attackers to gain access and potentially escalate their privileges.  


## Preparation

- Ensure proper licensing and certificates are present in the environment. An active Azure subscription and associated Entra tenant are needed to use Microsoft Entra Domain Services. 
- Ensure that account used to enable and synchronize authentication has Application Administrator, Groups Administrator, and Domain Services Contributor roles in the tenant.

## Risks

- Users must reset their passwords for the proper hashes to be stored by Entra Domain Services. Consider alerting users to the password changes to avoid help desk calls. 
- When associating an Azure subscription to a tenant, users who were assigned roles using Azure Role-Based Access Control (RBAC), service administrators and co-administrators will lose access. 
- This process is elaborate and resource-intensive, which may introduce additional risks or downtime during remediation of an incident. Moving to a managed domain may be a task for after adversary eviction. 

## Guidance

### Set up Domain Services (if not already set up)

- Associate Azure subscription with Microsoft Entra tenant.
- In the Microsoft Entra admin center, navigate to `Domain Services > Microsoft Entra Domain Services > Create Microsoft Entra Domain Services`.
- Select Azure subscription and resource group to be associated with the domain. Specify a Domain Name Server (DNS) name. 
- Choose Azure region for the managed domain and an appropriate SKU (can be changed later; if unsure, default to "Standard").
- Select "Review + create". All users in Microsoft Entra ID (formerly called Azure AD) will be synchronized into the managed domain. 

### Set up credential synchronization with Microsoft Entra ID

The process for synchronizing credentials is different for users in the cloud versus users in the on-premises directory. Ensure that synchronization is set up for all user groups.

- For cloud users, expire all passwords to force resets on log-in.
- For on-premises users, enable synchronization using the Synchronization Service in Microsoft Entra Connect. Repeat for each AD forest in the environment. 


## Associated Techniques

- [T1199](https://attack.mitre.org/techniques/T1199/) - Trusted Relationships

## Related Countermeasures

- CM0003 - Configure File and Directory Permissions
- CM0018 - Enable Active Directory (AD) Protected Users Security Group
- CM0063 - Investigate Suspicious Login Attempts
- CM0071 - Unenroll Suspicious Multifactor Authentication (MFA) Device from Microsoft User Account
- CM0079 - Reset Active Directory Services (AD DS) Connector Account Password
- CM0033 - Reset User Account Passwords
- CM0111 - Enable Multiple Administrative Approval (MAA) for Access Policies
- CM0142 - Enable LDAP Signing and Channel Binding
- CM0145 - Reset Interdomain Trust Account (ITA) Credentials

## References

- What is Microsoft Entra Domain Services? | <https://learn.microsoft.com/en-us/entra/identity/domain-services/overview>
- Tutorial: Create and configure a Microsoft Entra Domain Services managed domain | <https://learn.microsoft.com/en-us/entra/identity/domain-services/tutorial-create-instance>
- How objects and credentials are synchronized in a Microsoft Entra Domain Services managed domain | <https://learn.microsoft.com/en-us/entra/identity/domain-services/synchronization>
