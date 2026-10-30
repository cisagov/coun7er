# Enable Local Security Authority (LSA) Protections

* **ID:** CM0089
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows security; LSA protection and credential hardening

## Intended Outcome

Enabling Local Security Authority (LSA) protections blocks adversary credential access via Local Security Authority Subsystem Service (LSASS) process memory dumping.  

## Introduction

The Local Security Authority (LSA) is a process that validates users for sign-ins and enforces local security policies. LSA protection prevents code injection and memory reads by unprotected processes. Therefore, LSA protection prevents unauthorized access to the credentials stored and managed by the LSA.    

## Preparation

-   If the organization uses third-party authentication modules, these modules must be digitally signed with a Microsoft digital signature and be compliant with the Microsoft Security Development Lifecycle.
    -	Identify all LSA plug-ins and drivers, including non-Microsoft drivers or plug-ins and internally developed software. 
    -	Ensure all LSA plug-ins are digitally signed with a Microsoft certificate.
    -	Ensure all signed plug-ins successfully load into LSA and function correctly.
    -	Review audit-logs to identify LSA plug-ins and drivers that fail to run as protected processes. 

## Risks

-	If third-party modules do not meet these requirements, there is a risk of operational disruption.  

## Guidance

-	Use the Group Policy Editor to create a new Group Policy Object (GPO) that’s linked to the domain or the Organizational Unit (OU) that contains user accounts.
-	Create a new registry item in the HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa path and specify the value “RunAsPPL”.
-	Apply the policy, update group policies with `gpupdate /force`, and verify that it was successfully enabled.

## Associated Techniques

-	[T1003.002](https://attack.mitre.org/techniques/T1003/002/) – OS Credential Dumping: Security Account Manager
-	[T1003.004](https://attack.mitre.org/techniques/T1003/004/) – OS Credential Dumping: LSA Secrets
-	[T1003.005](https://attack.mitre.org/techniques/T1003/005/) – OS Credential Dumping: Cached Domain Credentials
-	[T1589.001](https://attack.mitre.org/techniques/T1589/001/) – Gather Victim Identity Information: Credentials


## Related Countermeasures

- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0018 - Enable Active Directory (AD) Protected Users Security Group
- CM0087 - Disable Credential Caching in the WDigest Authentication Protocol
- CM0088 - Enable Credential Guard
- CM0090 - Detect Attempts to Access Local Security Authority Subsystem Service (LSASS) Process
- CM0094 - Restrict Domain Admin User Rights and Monitor Protected Users on Workstations
- CM0096 - Prevent and Detect Attempts to Copy NT Directory Services Database Information Table (NTDS.dit)

## References

- Configure added LSA protection | <https://learn.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/configuring-additional-lsa-protection>
