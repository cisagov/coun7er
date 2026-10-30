# Reset Active Directory Services (AD DS) Connector Account Password

* **ID:** CM0079
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory/Azure AD Connect administration; connector accounts and password rotation

## Intended Outcome

Resetting the Active Directory Services (AD DS) Connector account password eliminates persistence and privilege escalation via hijacking of AD DS Connector account.

## Introduction

Entra Connect (formerly Azure AD Connect) is used to synchronize identity information, enabling users to be signed into both the organization's on-premises and cloud infrastructure. Entra Connect consists of three accounts:

* **AD DS Connector account:** Also referred to as the AD Connector, this account is used to read and write information to the on-premises Windows Server AD by using Active Directory Domain Services (AD DS). This account is a Microsoft Online Services (MSOL) account with a name starting with MSOL_ followed by a hex code (e.g. MSOL_4f374eb2b150). While it requires a complex password, by default its passwords are not set to expire. 
* **ADSync service account** (also called the sync engine): the main component of Entra Connect, this account is used to run the sync service and access the SQL Server database that stores Azure AD Connect's information and settings. It's default name is NT SERVICE\ADSync.
* **Microsoft Entra Connector account:** Formerly known as the AAD Connector Account, this account is used to write information to the Microsoft Entra ID (formerly Azure Active Directory or AAD) tenant of the organization's Microsoft 365 subscription. It is named Sync_[hostname of on-prem Entra Connect server]_[ID].

Entra Connect's AD DS Connector account possesses permissions that enable DCSync. 
* Replicating Directory Changes
* Replicating Directory Changes All

These permissions, usually reserved for domain admins and the domain controllers, facilitates password hash synchronization; AD DS Connector account can retrieve an NT hash for an account that changes its password and provide it to the Entra Connector Account.

However, if the AD DS Connector account is compromised, adversaries may use it to carry out DCSync attacks to dump secrets on a specified domain controller or move laterally to Azure AD.

## Preparation

- Identify the location of the AD DS Connector account within the on-prem Active Directory forest. If express settings were used, it will be created in Windows Server AD and located in the forest root domain in the Users container and begin with the prefix MSOL_.

## Risks

- Resetting an Entra Connect password may disrupt password synchronization between the on-prem AD and Microsoft Entra / Azure Active Directory. 

## Guidance

The following guidance describes the process of rotating the password associated with Entra Connect. Note that Microsoft Entra Connect cloud sync is replacing Microsoft Entra Connect Sync. Organizations should consider migrating to Cloud sync.

### Reset Entra Connect AD DS Account Password

To change the AD DS connector account in Active Directory, system administrators can use either the Active Directory Users and Computers Console (ADUC) or via PowerShell with the Set-ADAccountPassword cmdlet. 

### Update Microsoft Entra Connect Synchronization Service with New Password

System administrators should update Synchronization Service with the new account password. Otherwise synchronization will fail with a no-start-credentials error. 

From the Synchronization Service Manager, under the connections tab, select the AD Connector corresponding to the AD DS connector account. Under Actions, select properties and Connect to Active Directory Forest. Confirm the ID of the AD DS account and enter the new password in the corresponding box. Afterwards, restart Microsoft Entra ID Sync from the Windows Control Manager. 

The PowerShell cmdlet `Start-ADSyncSyncCycle -PolicyType Delta` can be used on the Entra Connect Server to sync changes which can be verified in the Synchronization Service Manager. 

### Lock Down AD DS Account

Lock down access to the AD DS account by updating the accounts permissions. For further information see https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/reference-connect-version-history-archive#lock

## Associated Techniques

- [T1078.004](https://attack.mitre.org/techniques/T1078/004/) - Valid Accounts: Cloud Accounts 
- [T1003.006](https://attack.mitre.org/techniques/T1003/006/) - OS Credential Dumping: DCSync 

## Related Countermeasures

- CM0028 - Reset Service Account Passwords
- CM0070 - Reset Domain Controller (DC) Machine Account Password
- CM0088 - Enable Credential Guard
- CM0092 - Reset Entra Seamless Sign-on Account Password
- CM0121 - Enable Managed Domain Authentication

## References

- Changing the AD DS connector account password | <https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-sync-change-addsacct-pass>
- Microsoft Entra Connect: Accounts and permissions | <https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/reference-connect-accounts-permissions>
- Guarding the Bridge: New Attack Vectors in Azure AD Connect | <https://www.sygnia.co/blog/guarding-the-bridge-new-attack-vectors-in-azure-ad-connect/>
- Abuse of Azure AD Connect Sync Service Account | <https://github.com/Cloud-Architekt/AzureAD-Attack-Defense/blob/main/AADCSyncServiceAccount.md>
- Change AD DS Connector account | <https://www.alitajran.com/change-ad-ds-connector-account/>
- Azure AD Connect for Red Teamers | <https://blog.xpnsec.com/azuread-connect-for-redteam/>
- Securing Microsoft Azure AD Connect | <https://www.hub.trimarcsecurity.com/post/securing-microsoft-azure-ad-connect>
- Protect Microsoft Entra Connect (Azure AD Connect) from hackers | <https://www.prosec-networks.com/en/blog/microsoft-entra-connect-azure-ad-connect-vor-hackern-schuetzen/>
