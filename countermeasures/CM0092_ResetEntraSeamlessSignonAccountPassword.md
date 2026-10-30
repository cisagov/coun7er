# Reset Entra Seamless Sign-on Account Password

* **ID:** CM0092
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Microsoft Entra ID administration; Seamless SSO accounts and password rotation

## Intended Outcome

Resetting the Microsoft Entra seamless sign-on account password terminates persistence and privilege escalation via hijacking of the seamless sign-on account.

## Introduction

Microsoft Entra seamless single sign-on (formerly Azure Active Directory (AD) Seamless Single Sign-On or Azure AD / AAD Seamless SSO) is a Microsoft Entra ID feature that automatically signs users in when they are on a on-premises domain joined device.

When Entra seamless SSO is first configured for an organization, a service principal named AzureADSSOACC is created in the on-premises AD and in each AD forest that synchronizes to Entra ID via Entra Connect. By default this account's password is not set to expire despite being particularly sensitive and should therefore be refreshed every 30 days. 

## Preparation

- Confirm that the organization has implemented Entra seamless SSO. Organizations may not be using Entra seamless SSO and instead use Windows Hello for Business or simply just Entra (formerly Azure AD) Hybrid Join.
- Ensure the system administrator rolling over the Kerberos decryption key possesses domain and global admin/hybrid identity administrator credentials.  

## Risks

- Removing the domain administrator account from the Protected Users group may place the domain administrator at greater risk of compromise. If using the Protected Users group for domain administrators, return the account back to the Protected Users group after rotating the Entra Seamless SSO Password. 

## Guidance

### Reset Entra Seamless SSO Password with Azure AD PowerShell

System administrators should follow Microsoft's documentation and use Azure AD PowerShell to reset the Entra seamless SSO password. The following are instructions for Azure AD PowerShell. Note that Microsoft is expected to depreciate Azure AD PowerShell and replace it with Microsoft Graph PowerShell in March 2024. 

1. Sign into the Entra Connect server with domain administrator account (note that if the domain administrator is a member of the Protected Users group, this operation will fail.)
2. Import the PowerShell module `AzureADSSO.psd1`
3. Run PowerShell as administrator and call `New-AzureADSSOAuthenticationContext` to sign in with Global administrator or Hybrid administrator credentials
4. Call `Get-AzureADSSOStatus | ConvertFrom-Json` to retrieve a list of AD forests where AD seamless SSO is enabled.
5. Call `Update-AzureADSSOForest`.
6. Call `$Cred = Get-Credential` 
7. Call `Update-AzureADSSOForest -OnPremCredentials $creds` to update the Kerberos decryption key for AZUREADSSO in an AD forest and Entra ID. 
8. Verify the password for AzureADSSOACC has been rotated.
9. Repeat this process for each AD forest with AD seamless SSO. Ensure these forests have connectivity to the global catalog server (TCP 3268 and TCP 3269). This does not need to be done on Entra Connect servers in staging mode. `Update-AzureADSSOForest` should not be run more than once per forest. 

## Associated Techniques

- [T1078](https://attack.mitre.org/techniques/T1078/) -  Valid Accounts 

## Related Countermeasures

-   CM0050 - Reset Kerberos Ticket Granting Ticket (KRBTGT) Password Twice
-   CM0028 - Reset Service Account Passwords
-   CM0079 - Reset Active Directory Services (AD DS) Connector Account Password

## References

- Microsoft Entra seamless single sign-on | <https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-sso>
- Microsoft Entra seamless single sign-on: Frequently asked questions | <https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-sso-faq#how-can-i-disable-seamless-sso->
- Az - Seamless SSO | <https://cloud.hacktricks.xyz/pentesting-cloud/azure-security/az-lateral-movement-cloud-on-prem/azure-ad-connect-hybrid-identity/seamless-sso>
- Remediation and Hardening Strategies for Microsoft 365 to Defend Against UNC2452 | <https://www.mandiant.com/sites/default/files/2021-10/Mandiant-unc2452-remediations-and-hardening-strategies-whitepaper.pdf>
