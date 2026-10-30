# Disable Credential Caching in the WDigest Authentication Protocol

* **ID:** CM0087
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows authentication; WDigest and credential caching

## Intended Outcome

Disabling credential caching in the WDigest authentication protocol restricts adversary credential access and privilege escalation.

## Introduction

WDigest is a legacy authentication protocol that stores clear-text passwords in memory. WDigest is frequently targeted by adversaries who perform credential dumping in order to escalate privileges and move laterally.  As of the release of Windows Server 2012 R2, WDigest should be disabled by default on recent versions of the Windows operating system. However, it is not removed and can be enabled by an adversary with local administrator privileges. Disabling “UseLogonCredential” prevents WDigest from storing clear-text passwords in memory.   
  

## Preparation

-	Determine if and to what extent the organization uses WDigest for authentication. This can be accomplished by reviewing event logs on servers for event ID 4624 and checking domain controller logs for event ID 4776 to identify whether any users have logged in with ‘Authentication Package: WDigest’.
- Verify that WDigest has not been manually enabled. The status of WDigest can be queried using 

    `reg query HKLMSYSTEMCurrentControlSetControlSecurityProvidersWDigest /v UseLogonCredential`

- Update KB2871997 must first be installed to disable WDigest authentication in Windows 7, Windows 8, Windows Server 2008 R2 and Windows Server 2012.

## Risks

-	Organizations that use WDigest for authentication will experience operational disruption if they prevent WDigest from storing clear-text credentials.

## Guidance

-	Use the Group Policy Editor to create a new Group Policy Object (GPO) that’s linked to the domain or the Organizational Unit (OU) that contains user accounts.
-	Set the “UseLogonCredential” registry key in the HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest path to “0.” 
-   Force GPO update with `gpupdate`
-	Verify the change persists.

## Associated Techniques

-	[T1003.001](https://attack.mitre.org/techniques/T1003/001/) – OS Credential Dumping: LSASS Memory

## Related Countermeasures

- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0088 - Enable Credential Guard
- CM0089 - Enable Local Security Authority (LSA) Protections
- CM0090 - Detect Attempts to Access Local Security Authority Subsystem Service (LSASS) Process
- CM0094 - Restrict Domain Admin User Rights and Monitor Protected Users on Workstations

## References

- Disabling credentials caching in WDigest | <https://learn.microsoft.com/en-us/answers/questions/463368/disabling-credentials-caching-in-wdigest>
- An Overview of KB2871997 | <https://msrc.microsoft.com/blog/2014/06/an-overview-of-kb2871997/>
- WDigest Clear-Text Passwords: Stealing More than a Hash | <https://blog.netwrix.com/2022/10/11/wdigest-clear-text-passwords-stealing-more-than-a-hash/>
