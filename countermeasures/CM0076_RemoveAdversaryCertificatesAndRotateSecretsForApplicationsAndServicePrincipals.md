# Remove Adversary Certificates and Rotate Secrets for Applications and Service Principals 

* **ID:** CM0076
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Cloud identity administration; certificates, secrets, and service principals

## Intended Outcome

Removing adversary certificates and rotating secrets for applications and service principals terminates persistence via adversary-controlled credentials.

## Introduction

Each application in Microsoft Entra ID is associated with a service principal and an application object.

- Service principals, also referred to as Enterprise Applications in the Azure Portal, are security identities or accounts used by applications in a tenant or directory to interact with Microsoft Azure via the Azure API. Service principals are local representations in a tenant and are based on application objects. 

- Application objects, (managed as App Registrations in the Azure Portal) are the global representation of an application across tenants and serve as a template for service principal objects. 

To authenticate to Microsoft 365 and access the API, applications have credentials (x509 certificates or secrets) associated with them.

Adversaries may add their own credentials to a service principal or application object to maintain persistent access to victim accounts and instances within the environment. Adversaries that add new certificates or secrets to an application object or service principal will possess the ability to perform API operations using the permissions that are assigned to it. By hijacking service principals and applications, adversaries can inherit permissions that enable them to access data normally protected by multi-factor authentication (MFA), such as the contents of a mailbox via Mail.Read, Mail.ReadWrite permissions.

Sophisticated adversaries may remove any adversary-controlled credentials after carrying out their objectives. Organizations should nonetheless investigate any lingering adversary-controlled credentials and increase security and monitoring of this attack technique in their cloud environment.

## Preparation

- Connect to Azure and enumerate application objects and service principals, flagging those which are suspect. The identification of permission and credential changes to applications and service principals can be accelerated using [Sparrow](https://github.com/cisagov/Sparrow). Alternately, responders should investigate the existing capabilities of their SIEM and/or the applicability of [similar](https://github.com/mandiant/Mandiant-Azure-AD-Investigator) [forensic](https://cloudforensicator.com/) [tools](https://github.com/AzureAD/Azure-AD-Incident-Response-PowerShell-Module) to assist with detection and investigation of applications and service principals in Azure. Validate that an application or service principal is likely compromised by considering a service principal's creation date, user account that provided consent, and the permissions associated with the service principal. Consider whether it has illustrated any suspicious sign-on behavior, including login location, failures, timestamps, and frequency. Review Entra ID Audit log and Unified Audit logs. A compromised application object used for applications from additional organizations means an adversary will have access to these organization's data as well. 

- Accounts used to remove certificates and passwords from application and service principals should have the appropriate permissions, such as global administrator or security administrator.

- Because credential rotation for service principals is common as service principal certificates expire upon completion of their lifespan, leaning on existing change control processes to document rotation and distinguish valid from unusual credential rotation may help investigation and response.

## Risks

- Removing credentials may prevent services from authenticating, inhibiting their functionality and thus disrupting business operations. Assess business impact prior to removing any credentials.

- Revoking refresh tokens will cause users to need to reauthenticate, risking additional disruption to business operations. 

## Guidance

This countermeasure assumes that a suspicious service principal or application object has been identified and a compromise has occurred. 

### Managing Application and Service Principal Certificates and Rotating Secrets in Microsoft Azure

Service principals can be created, removed, and modified by using a number of different tools, including Microsoft Graph PowerShell, Graph API, Azure PowerShell, the Azure command-line interface (CLI), and the deprecated AzureAD PowerShell module.

Note that if an application object is modified, the changes will only show in its service principal object in the home tenant (where it was registered).

###  via Entra Admin Center

Application registrations and service principal credentials may be added and/or removed within the Microsoft Entra Admin Center under "Certificates & secrets."

####  via Microsoft Graph API

Credentials can be added and/or removed using the Graph API.

- [`servicePrincipal: removePassword`](https://learn.microsoft.com/en-us/graph/api/serviceprincipal-removepassword?view=graph-rest-1.0&tabs=http) may be used to remove a secret from a service principal object.

- [`servicePrincipal: addPassword`](https://learn.microsoft.com/en-us/graph/api/serviceprincipal-addpassword?view=graph-rest-1.0&tabs=http) may be used to add a secret to a service principal

Combining both effectively rotates the secret for the associated service principal. 

- [`servicePrincipal: removeKey`](https://learn.microsoft.com/en-us/graph/api/serviceprincipal-removekey?view=graph-rest-1.0&tabs=http) may be used to remove a key from a Service Principal. 

- [`servicePrincipal: addKey`](https://learn.microsoft.com/en-us/graph/api/serviceprincipal-addkey?view=graph-rest-1.0&tabs=http) may be used to add a key to a Service Principal. 

Combining these requests effectively rotates a specific service principal key.

#### via Azure CLI

- [`az ad sp credential reset`](https://learn.microsoft.com/en-us/cli/azure/ad/sp/credential?view=azure-cli-latest#az-ad-sp-credential-reset) can be used to reset a service principals password or certificate credential. 

- [`az ad sp credential delete`](https://learn.microsoft.com/en-us/cli/azure/ad/sp/credential?view=azure-cli-latest#az-ad-sp-credential-delete) can be used to remove a secret or certificate credential. 

#### via Azure PowerShell module

- [`Remove-AzADAppCredential`](https://learn.microsoft.com/en-us/PowerShell/module/az.resources/remove-azadappcredential?view=azps-0.10.0) can be used to remove credentials associated with one or more service principals.

- [`Remove-AzADApplication`](https://learn.microsoft.com/en-us/powershell/module/az.resources/remove-azadapplication?view=azps-11.6.0) deletes a specific Azure AD application.

- [`Remove-AzADSpCredential`](https://learn.microsoft.com/en-us/powershell/module/az.resources/remove-azadspcredential?view=azps-11.6.0) removes credentials associated with one or more service principal.

- [`Remove-AzADServicePrincipal`](https://learn.microsoft.com/en-us/powershell/module/az.resources/remove-azadserviceprincipal?view=azps-11.6.0) deletes a specific service principal.

### Revoke All Refresh Tokens

After removing adversary-controlled certificates and rotating secrets associated with applications and service principals, all associated refresh tokens should be revoked. Like the prior instructions, this may be accomplished through multiple channels, such as Graph or the Azure PowerShell module. 

####  via Microsoft Graph API

- [`user: invalidateAllRefreshTokens`](https://learn.microsoft.com/en-us/graph/api/user-invalidateallrefreshtokens?view=graph-rest-beta&tabs=http) revokes all refresh tokens issued to applications (beta version as of May 2024)

#### via Azure PowerShell module

- [`Revoke-AzureADUserAllRefreshToken`](https://learn.microsoft.com/en-us/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0) revokes existing refresh tokens.

#### Verify that Application Objects and Service Principals are removed from environment

As part of remediation, if any application objects or service principals are removed from the environment, confirm they are removed and monitor for unexpected changes to applications in Azure.

## Associated Techniques

- [T1098](https://attack.mitre.org/techniques/T1098/) - Account Manipulation
- [T1098.001](https://attack.mitre.org/techniques/T1098/001/) -  Account Manipulation: Additional Cloud Credentials 

## Related Countermeasures

- CM0105 - Remove Malicious Enterprise Applications and Service Account Principals
- CM0075 - Implement Access Restrictions on Cloud Storage Objects
- CM0077 - Revoke Microsoft 365 Refresh Tokens
- CM0082 - Identify and Remove Suspicious Email Forwarding Rules
- CM0083 - Rotate Application Programming Interface (API) Keys
- CM0032 - Revoke and Regenerate SSH Keys
- CM0099 - Remove/Rotate Kubernetes Service Account Token
- CM0134 - Audit Federated Systems Trust Settings

## References

- Application and service principal objects in Microsoft Entra ID | <https://learn.microsoft.com/en-us/entra/identity-platform/app-objects-and-service-principals>
- Securing service principals in Microsoft Entra ID | <https://learn.microsoft.com/en-us/entra/architecture/service-accounts-principal>
- Microsoft Entra security operations guide for applications | <https://learn.microsoft.com/en-us/entra/architecture/security-operations-applications>
- How adversaries use Entra ID service principals in business email compromise schemes | <https://redcanary.com/blog/threat-detection/entra-id-service-principals/>
- Compromised and malicious applications investigation | <https://learn.microsoft.com/en-us/security/operations/incident-response-playbook-compromised-malicious-app>
- Remediation and Hardening Strategies for Microsoft 365 to Defend Against UNC2452 | <https://www.mandiant.com/sites/default/files/2021-11/wp-m-unc2452-000343.pdf>
- Microsoft Entra recommendation: Renew expiring service principal credentials | <https://learn.microsoft.com/en-us/entra/identity/monitoring-health/recommendation-renew-expiring-service-principal-credential>
- Microsoft Entra recommendation: Renew expiring application credentials | <https://learn.microsoft.com/en-us/entra/identity/monitoring-health/recommendation-renew-expiring-application-credential>
- Sparrow | <https://github.com/cisagov/Sparrow>
- Mandiant Azure AD Investigator | <https://github.com/mandiant/Mandiant-Azure-AD-Investigator>
- Cloud Forensics using Hawk | <https://cloudforensicator.com/>
- Azure AD Incident Response PowerShell Module | <https://github.com/AzureAD/Azure-AD-Incident-Response-PowerShell-Module>
- servicePrincipal: removePassword | <https://learn.microsoft.com/en-us/graph/api/serviceprincipal-removepassword?view=graph-rest-1.0&tabs=http>
- servicePrincipal: addPassword | <https://learn.microsoft.com/en-us/graph/api/serviceprincipal-addpassword?view=graph-rest-1.0&tabs=http>
- servicePrincipal: removeKey | <https://learn.microsoft.com/en-us/graph/api/serviceprincipal-removekey?view=graph-rest-1.0&tabs=http>
- servicePrincipal: addKey | <https://learn.microsoft.com/en-us/graph/api/serviceprincipal-addkey?view=graph-rest-1.0&tabs=http>
- az ad sp credential reset | <https://learn.microsoft.com/en-us/cli/azure/ad/sp/credential?view=azure-cli-latest#az-ad-sp-credential-reset>
- az ad sp credential delete | <https://learn.microsoft.com/en-us/cli/azure/ad/sp/credential?view=azure-cli-latest#az-ad-sp-credential-delete>
- Remove-AzADAppCredential | <https://learn.microsoft.com/en-us/PowerShell/module/az.resources/remove-azadappcredential?view=azps-0.10.0>
- Remove-AzADApplication | <https://learn.microsoft.com/en-us/powershell/module/az.resources/remove-azadapplication?view=azps-11.6.0>
- Remove-AzADSpCredential | <https://learn.microsoft.com/en-us/powershell/module/az.resources/remove-azadspcredential?view=azps-11.6.0>
- Remove-AzADServicePrincipal | <https://learn.microsoft.com/en-us/powershell/module/az.resources/remove-azadserviceprincipal?view=azps-11.6.0>
- user: invalidateAllRefreshTokens | <https://learn.microsoft.com/en-us/graph/api/user-invalidateallrefreshtokens?view=graph-rest-beta&tabs=http>
- Revoke-AzureADUserAllRefreshToken | <https://learn.microsoft.com/en-us/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0>
