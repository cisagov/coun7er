# Remove Malicious Enterprise Applications and Service Account Principals

* **ID:** CM0105
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Eliminate
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Microsoft Entra ID administration; Azure Portal administration, Graph API administration

## Intended Outcome

Removing malicious applications and service account principals blocks an adversary's persistence via applications and service principals in the environment. 

## Introduction

Adversaries may compromise legitimate enterprise applications with malicious code using hooks, payload injections, and other tactics to maintain persistence and evade defenses. Service principals are the identities of applications as stored in Entra ID, determining the resources an application is allowed to access. Compromised service principals lead to incorrect access controls and permissions. 

Malicious Azure applications are a newer attack vector for adversaries since they are difficult to block and detect in an environment. Adversaries create a custom Azure application to use in a phishing attack, then use the Azure APIs to integrate with a victim's Microsoft 365 environment, obtain persistence, remotely execute code, or perform discovery on the organization. These applications do not require permission from Microsoft, come with valid Microsoft certificates, and they do not require code execution on endpoint devices, making detection via antivirus (AV) solutions or endpoint detection and response (EDR) difficult. 

While a user is informed that an application is not published by Microsoft or their organization when they are prompted for authorization and permissions, this may be overlooked. Upon gaining access, attackers can access files, read and send emails, modify the calendar, and see all other users in the organization via user directories. 

## Preparation

- Audit Enterprise Applications in the Azure portal to ensure all applications are known or trusted.
- Identify the impact removing the applications or service account principles will have on business operations.
- Identify if privileged or administrator credentials are compromised due to the malicious application and coordinate credential resets or other eviction techniques to eliminate all adversary access to/through the application.
- Determine the ID of the application(s) to be removed.
- Locate accounts using service principals and identify which ones are legitimate. Both Azure command line interface (CLI) and PowerShell have commands to discover service principled accounts. 

## Risks

- Deleting legitimate applications can be disruptive to business operations; audit if applications are in use before deleting and alert users. 
- Removing service principals can cause users to no longer have access to applications. 
- Microsoft discourages organizations from completely disabling third-party applications. Instead, Microsoft recommends auditing 3rd party applications used by the organization and removing any that are not critical to business operations or have misconfigured permissions.

## Guidance

Authentication of the application is handled by Microsoft and users log in with valid credentials to their Office 365 instance, so multi-factor authentication (MFA) is not a useful mitigation for this tactic. Malicious applications should be deleted or disabled with revoked permissions, though deletion is recommended.  

### Deleting applications through Azure portal or Microsoft Graph API

- Via the Azure portal, delete applications in the `Enterprise Applications` section under the `Azure Active Directory` tab.
- Via the Graph API, soft delete the application (recoverable for 30 days) using `DELETE /applications{id}`.
	- If you do not permanently delete the application, set up monitoring or logging to be alerted if the application state changes, such as being reenabled or recovered. 
- Permanently delete the application using `DELETE /directory/deletedItems/{id}`.

### Remediate service principals

- Rotate any KeyVault secrets that the service principal had access to in the following order:
	- Directly exposed secrets via the `GetSecrets` calls.
	- Other exposed secrets in KeyVault.
	- Other exposed secrets across other subscriptions. 
- Remove service principals for malicious applications using the `Remove-ServicePrincipal` cmdlet, specifying the ID of the service principal. 

## Associated Techniques

- [T1554](https://attack.mitre.org/techniques/T1554/) - Compromise Host Software Binary


## Related Countermeasures

- CM0022 - Quarantine Suspicious Files
- CM0076 - Remove Adversary Certificates and Rotate Secrets for Applications and Service Principals
- CM0135 - Rebuild Group Managed Service Accounts (gMSA)


## References

- Compromised and malicious applications investigation | <https://learn.microsoft.com/en-us/security/operations/incident-response-playbook-compromised-malicious-app>
- Use service principals & managed identities | <https://learn.microsoft.com/en-us/azure/devops/integrate/get-started/authentication/service-principal-managed-identity?view=azure-devops>
- Securing service principals in Microsoft Entra ID | <https://learn.microsoft.com/en-us/entra/architecture/service-accounts-principal>
- Remove-ServicePrincipal | <https://learn.microsoft.com/en-us/powershell/module/exchange/remove-serviceprincipal?view=exchange-ps>
