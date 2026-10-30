# Revoke Microsoft 365 Refresh Tokens

* **ID:** CM0077
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Microsoft 365 administration; tokens, sessions, and identity revocation

## Intended Outcome

Revoking Microsoft 365 (M365) refresh tokens terminates adversary lateral movement and defensive evasion using compromised tokens upon expiration of their access token.

## Introduction

M365 refresh tokens are used by Microsoft 365 to request new access tokens to enable authenticated users to remain signed-in. Unlike refresh tokens, M365 access tokens cannot be revoked. They will expire according to their specified time (default being 1 hour). 

## Preparation

- This action should always be performed after issuing new Active Directory Federation Server (AD FS) certificates following a reported incident. 
- Appropriate permissions must be delegated in order to execute the cmdlets. In most cases these should be run with M365 `Global Administrator` permissions to revoke refresh tokens for all roles.

## Risks

- This countermeasure may disrupt operations.
- User accounts will be required to reauthenticate to M365.

## Guidance

Refresh tokens in Microsoft 365 can be revoked either with the AD PowerShell module or Graph API. 

### Revoke M365 Refresh Tokens via Graph API

Administrators can use `Get-MgUser - All` with  `Revoke-MgUserSignInSession` to invalidate all refresh tokens.

### Revoke M365 Refresh Tokens via Azure AD PowerShell Module

First, connect to Azure AD with `Connect-AzureAD`

Afterwards, administrators may run the following command to revoke M365 refresh tokens.

`Get-AzureADuser -all $true | Revoke-AzureADUserAllRefreshToken`

## Associated Techniques

- [T1550](https://attack.mitre.org/techniques/T1550/) -  Use Alternate Authentication Material  
- [T1550.001](https://attack.mitre.org/techniques/T1550/001/) -   Use Alternate Authentication Material: Application Access Token 

## Related Countermeasures

- CM0050 - Reset Kerberos Ticket Granting Ticket (KRBTGT) Password Twice
- CM0069 - Reset Directory Services Restore Mode (DSRM) Account Passwords on Domain Controllers
- CM0073 - Rotate Active Directory Federation Services (AD FS) Certificates
- CM0076 - Remove Adversary Certificates and Rotate Secrets for Applications and Service Principals
- CM0083 - Rotate Application Programming Interface (API) Keys
- CM0099 - Remove/Rotate Kubernetes Service Account Token

## References

- Revoke-AzureADUserAllRefreshToken | <https://learn.microsoft.com/en-us/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0>
- Immediately revoke access to Microsoft 365 applications | <https://www.michev.info/blog/post/4688/immediately-revoke-access-to-microsoft-365-applications>
- Find Azure AD PowerShell and MSOnline cmdlets in Microsoft Graph PowerShell | <https://learn.microsoft.com/en-us/powershell/microsoftgraph/azuread-msoline-cmdlet-map?view=graph-powershell-1.0>
