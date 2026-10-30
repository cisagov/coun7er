# Restrict Domain Admin User Rights and Monitor Protected Users on Workstations

* **ID:** CM0094
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; Domain Admin rights and Protected Users

## Intended Outcome

Restricting Domain Admin user rights and monitoring Protected Users on workstations and servers limits the likelihood of privileged credential access via credential dumping.

## Introduction

Adversaries iteratively dump credentials and move laterally to progressively escalate privileges.  Domain Administrator credentials are often the target of these efforts as they provide an unparalleled level of privilege in the Windows environment.  This countermeasure will prevent Domain Administrator credentials from being cached on member workstations and servers and therefore reduce the likelihood of Domain Administrator credential access.

## Preparation

-	Review all accounts (user and service) in the Domain Administrator group.  Ensure that all accounts added to the Domain Administrator group require Domain Administrator privileges.  
-	Domain Administrators are default members of the local Administrator group on all member workstations and servers to enable build and disaster recovery scenarios. Consider removing all members from the Domain Administrators group.  You may elect to allow the local Administrator group if it has been appropriately secured in accordance with Microsoft guidance.   

## Risks

Identify scenarios in which Domain Administrators are currently accessing member workstations and servers and consider alternative courses of action to perform these tasks in a manner that does not introduce vulnerability and to minimize the likelihood of operational interruption.  

## Guidance

### Configure

-	Create a new Group Policy Object (GPO) or modify an existing one.
-	Navigate to `Configuration > Windows Settings > Security Settings > Local Policies > User Rights Assignment`.
-	Add the Domain Administrators group to the following list:
    - `Deny access to this computer from the network`
    - `Deny log on as a batch job`
    - `Deny log on as a service`
    - `Deny log on locally`
    - `Deny log on through Remote Desktop Service user rights`
-	Apply the GPO to the appropriate Organizational Units (OU).

### Monitor

-	Consider filtering and aggregating Windows event logs to monitor protected users and interactive logons `event id 4624 – type 2`.

## Associated Techniques

-	[T1003](https://attack.mitre.org/techniques/T1003) – OS Credential Dumping

## Related Countermeasures

- CM0059 - Configure Tactical Privileged Access Workstation
- CM0087 - Disable Credential Caching in the WDigest Authentication Protocol
- CM0088 - Enable Credential Guard
- CM0089 - Enable Local Security Authority (LSA) Protections
- CM0090 - Detect Attempts to Access Local Security Authority Subsystem Service (LSASS) Process

## References

- Appendix F: Securing Domain Admins Groups in Active Directory | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/appendix-f--securing-domain-admins-groups-in-active-directory>
- Appendix D: Securing Built-In Administrator Accounts in Active Directory | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/appendix-d--securing-built-in-administrator-accounts-in-active-directory>
