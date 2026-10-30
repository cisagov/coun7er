# Remove Malicious Domain Trust from Tenant

* **ID:** CM0123
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Eliminate
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Identity administration; domain trust relationships and malicious trust removal

## Intended Outcome

Removing malicious domain trusts from the tenant prevents an adversary from achieving initial access or performing discovery by abusing trusted domains. 

## Introduction

A domain trust is a bridge between two separate domains or forests that allows users from the two domains to access shared resources by allowing authentication traffic to flow between them. Trust relationships between domains allow the requested domain to see the requestor as a service, and not validate the user asking for access as vigorously (it trusts the domain that issued the referral). 

Many organizations still maintain trusts established years ago, which opens them up to adversaries who may compromise external trusted domains. Adversaries often leave implants and malicious code in the often less-secure trusted domains for future re-compromise instead of leaving traces in the main target domain. Domain trusts also allow adversaries to move laterally between domains via compromised accounts, escalating their privileges until they reach their target. 

## Preparation

- Enumerate all trusts in the current domain, including trusts that the trusted domains have; create a map of domain trusts.
- Audit resources provided by domain trusts. 
- Ensure the account performing domain trust operations has at least Domain Administrator permissions. 

## Risks

- Removing domain trusts may cause users to lose access to shared resources. Consider auditing resources provided by a shared trust and weighing risk of continued trust versus recompromise. 

## Guidance

### Remove Domain Trust from On-Premises Domains

- On a management workstation for the domain, navigate to `Start > Administrative Tools > Active Directory Domains and Trusts`.
- Select the domain name, then navigate to `Properties > Trusts`.
	- To remove incoming trusts, select `Domains that trust this domain (incoming trusts)`, select the trust, and select `Remove`.
	- To remove outgoing trusts, select `Domains trusted by this domain (outgoing trusts)` select the trust, and select `Remove`.

### Remove Domain Trust from Domain Services

- In the Microsoft Entra admin center, navigate to `Microsoft Entra Domain Services` and select the domain.
- Select `Trusts`, select the appropriate trust to remove, and select `Remove`. A password may be required. 

### Monitor for Domain Trust Enumeration Techniques

- Process calls to BloodHound or SharpHound.
- .NET scripts containing `([System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()).GetAllTrustRelationships()` or `([System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest()).GetAllTrustRelationships()`.
- Win32 API calls containing `DsEnumerateDomainTrusts()`.
- Process command lines containing `objectClass=trustedDomain` or `Get-DomainTrust`.

## Associated Techniques

- [T1482](https://attack.mitre.org/techniques/T1482) - Domain Trust Discovery
- [T1199](https://attack.mitre.org/techniques/T1199) - Trusted Relationship

## Related Countermeasures

- CM0023 - Identify and Terminate Suspicious Processes
- CM0063 - Investigate Suspicious Login Attempts
- CM0031 - Monitor PowerShell Program
- CM0134 - Audit Federated Systems Trust Settings
- CM0145 - Reset Interdomain Trust Account (ITA) Credentials

## References

- A Guide to Attacking Domain Trusts | <https://harmj0y.medium.com/a-guide-to-attacking-domain-trusts-ef5f8992bb9d>
- How Domain and Forest Trusts Work | <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc773178(v=ws.10)>
- How trust relationships work for forests in Active Directory | <https://learn.microsoft.com/en-us/entra/identity/domain-services/concepts-forest-trust>
- Trust Technologies | <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc759554(v=ws.10)>
- Remove a Trust | <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771137(v=ws.11)>
- Tutorial: Create a two-way forest trust in Microsoft Entra Domain Services with an on-premises domain | <https://learn.microsoft.com/en-us/entra/identity/domain-services/tutorial-create-forest-trust>
