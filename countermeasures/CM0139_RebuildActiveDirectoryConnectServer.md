# Rebuild Active Directory (AD) Connect Server

* **ID:** CM0139
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory/Entra Connect administration; server rebuilding and identity synchronization

## Intended Outcome

Rebuilding the Active Directory (AD) Connect Server prevents an adversary's attempts to move laterally, escalate privileges, or cause destruction by abusing the AD Connect servers. 

## Introduction

AD Connect servers (also may be referred to as Azure AD Connect or Entra Connect) provide synchronization services between on-premises and cloud identities in an organization. They allow for users in on-premises domains to use their already established credentials to access resources in the cloud and Microsoft Entra ID. AD Connect servers set up for older solutions such as DirSync may be valuable targets to attackers, as the accounts would have Global Administrator permissions. 

Adversaries can abuse AD Connect synchronization services to control the domain's services and perform destructive actions such as deleting resources. Adversaries can also gain access to email services via the compromised accounts and perform email impersonation to further their actions on objectives. Additionally, misconfigured or compromised AD Connect servers may allow attackers to use man-in-the-middle and pass-the-hash attacks to intercept and abuse legitimate user credentials. 

## Preparation

- Identify any backup synchronization options to use during downtime. 
- Ensure the account from which the server is being rebuilt has at least Global Administrator privileges for the domain. 

## Risks

- Removing synchronization from an environment can take up to 72 hours. Consider rebuilding the server during weekends or other long periods of low traffic hours. 
- Rebuilding a sync server takes several hours and synchronization services will be unavailable during that time. Though backup synchronization may be set up on other servers, consider rebuilding the server during lower traffic hours. 

## Guidance

### Remove current AD Connect Server

- Disable directory synchronization
	- If the Microsoft Azure Active Directory PowerShell module is not installed, install it with the `Install-Module MSOnline` command. Ensure PowerShell is run with administrator privileges. 
	- Run `Set-MsolDirSyncEnabled -EnableDirSync $false` to disable synchronization services. This may take upwards of 72 hours. 
	- Periodically run `(Get-MSOLCompanyInformation).DirectorySynchronizationEnabled` to check if the service has been disabled; when it is, the command will return `False`. 
- Uninstall AD Connect from the on-premises server. 

### Reinstall AD Connect Server

AD Connect can be installed two ways: express, or custom. Express installation can be used if the organization has a singe on-premises AD forest, an enterprise administrator account for the installation, and less than 100,000 objects. If any of those criteria is not met, a custom install must be done. 

#### Express Install

- Select `Use express settings` in the installation wizard and enter administrator and hybrid credentials as prompted.
- Select sign-in configuration as required by the organization. 
- The installation will automatically:
	- Install synchronization engine.
	- Configure AD Connector.
	- Configure ad.local Connector. 
	- Enable Password hash synchronization.
	- Enable Auto Upgrade.
	- Configure synchronization settings on the machine it is installing onto. 
- In the Azure management portal, update the user principal name (UPN) by selecting all the users, navigating to `Properties` and changing the UPN. 
- Run `Start-ADSyncSyncCycle -PolicyType Delta -Verbose` in PowerShell to start synchronization. 

#### Custom Install

- On the Express Settings page, select `Customize`.
- Entra Connect will automatically install synchronization services and assign appropriate groups and permissions, but allows for customization of installation location, using an existing SQL server, using an existing service account, specifying sync groups, or importing synchronization settings. These checkboxes may remain empty if the default values are sufficient. 
- Click `Install`. 
- Select the user sign-in method the organization plans on using. 
- Connect to Microsoft Entra ID with Hybrid Identity Administrator credentials when prompted. 
- Synchronize users with Entra ID. 
	- Connect directories by providing AD Connect with the appropriate forest name and credentials of a privileged account. Select `Add Directory`. 
	- In the dialog window, select the account option (create new AD account or use an existing one). Enter administrator credentials when prompted. 
	- Follow remaining dialog instructions regarding user configuration, domain exclusion, and optional features. 

## Associated Techniques

- [T1485](https://attack.mitre.org/techniques/T1485) - Data Destruction
- [T1534](https://attack.mitre.org/techniques/T1534) - Internal Spearphishing

## Related Countermeasures

- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts
- CM0107 - Restore SYSVOL Content in AD Domains
- CM0114 - Enable Microsoft Conditional Access Policy Templates
- CM0125 - Reset and Monitor adminCount Attribute

## References

- Microsoft Entra Connect: Staging server and disaster recovery | <https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-sync-staging-server>
- Can't manage or remove objects that were synchronized through the Azure Active Directory Sync tool | <https://learn.microsoft.com/en-us/troubleshoot/entra/entra-id/user-prov-sync/cannot-manage-objects>
- Select which installation type to use for Microsoft Entra Connect | <https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-install-select-installation>
- Custom Installation of Microsoft Entra Connect | <https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-install-custom>
