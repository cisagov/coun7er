# Audit Federated Systems Trust Settings

* **ID:** CM0134
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active
* **Nice Work Role:** Technology Program Auditing (OG-WRL-016)
* **Skills & Knowledge:** Identity security assessment; federation trust settings and permission auditing

## Intended Outcome

Auditing federated systems trust settings prevents an adversary's attempts at initial access, lateral movement, and persistence by abusing permissions or misconfigurations of federated trusts. 

## Introduction

A federated trust between two or more entities allows them to accept credentials from each other and grant users in both entities access to specific resources (can be calendars, files, etc.), or to enable single sign-on (SSO) for web services. 

Adversaries may edit trust settings to gain access to systems within the trust network, and then further abuse native Microsoft functionality to remain undetected in an environment. Additionally, they can move laterally from a compromised tenant to any other tenants within the trust network. 

## Preparation

- Identify all federated systems and services in the environment. 
- Identify policies for access permissions and expected trust settings for each trust. 
- Ensure there is enough storage for audit logs.

## Risks

- Removing trusts or decreasing access permissions between trusted entities may cause some users to lose access to resources they need to perform their jobs. Consider what recourses a trust grants and what users may need to access those resources, and weigh the risks of compromise against costs to business operations. 

## Guidance

In most environments, federated trust settings will be updated infrequently and should not cause a large amount of logs. Mandiant provides an auditing script to enumerate all domains, including unverified domains and those configured for federation. The script is available on their GitHub at <https://github.com/mandiant/Mandiant-Azure-AD-Investigator>.

### Check Trust Settings

- Audit all federated domains and ensure that permissions and settings are set to expected values. 
	- Enumerate all domains.
	- Remove suspicious or unexpected entries.

### Monitor Changes to Settings

- Configure Microsoft Entra audit logs for an Azure Log Analytics Workspace.
- Create alerts to trigger based on the log query. An example query is provided below:

	```
	AuditLogs 
	| extend TargetResource = parse_json(TargetResources) 
	| where ActivityDisplayName contains "Set federation settings on domain" or ActivityDisplayName contains "Set domain authentication" 
	| project TimeGenerated, SourceSystem, TargetResource[0].displayName, AADTenantId, OperationName, InitiatedBy, Result, ActivityDisplayName, ActivityDateTime, Type
	```
    
- Add action group to ensure notification when alerting conditions are met. 
- Monitor any changes to the `signingCertificate` attribute. Changing the attribute may temporarily disable authentication, making the attack visible to defenders. 
- Monitor for secondary or additional token-signing certificates added into the federation configuration. 
- Monitor for changes to Security Assertion Markup Language (SAML) tokens such as extended lifetime. 

## Associated Techniques

- [T1199](https://attack.mitre.org/techniques/T1199/) - Trusted Relationship 
- [T1484.002](https://attack.mitre.org/techniques/T1484/002/) - Domain or Tenant Policy Modification: Domain Trust Modification
- [T1606.002](https://attack.mitre.org/techniques/T1606/002/) - Forge Web Credentials: SAML Tokens

## Related Countermeasures

- CM0073 - Rotate Active Directory Federation Services (AD FS) Certificates
- CM0076 - Remove Adversary Certificates and Rotate Secrets for Applications and Service Principals
- CM0123 - Remove Malicious Domain Trust from Tenant
- CM0145 - Reset Interdomain Trust Account (ITA) Credentials

## References

- Federated Trust | <https://csrc.nist.gov/glossary/term/federated_trust>
- What is federation with Microsoft Entra ID? | <https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/whatis-fed>
- Monitor changes to federation configuration in your Microsoft Entra ID | <https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-monitor-federation-changes>
- Remediation and Hardening Strategies for Microsoft 365 to Defend Against UNC2452 | <https://www.mandiant.com/sites/default/files/2021-11/wp-m-unc2452-000343.pdf#page=23>
