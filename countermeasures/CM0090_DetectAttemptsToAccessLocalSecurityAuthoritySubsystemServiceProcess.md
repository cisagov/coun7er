# Detect Attempts to Access Local Security Authority Subsystem Service (LSASS) Process

* **ID:** CM0090
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active
* **Nice Work Role:** Defensive Cybersecurity (PD-WRL-001)
* **Skills & Knowledge:** Security monitoring; LSASS access and credential-dumping detection

## Intended Outcome

Detecting attempts to access the Local Security Authority Subsystem Service (LSASS) process detects adversary credential access via LSASS credential dumping.

## Introduction

Local Security Authority Subsystem Service (LSASS) is a crucial system file on the Microsoft Windows operating system that is responsible for verifying users logging into a system, handling password changes, and creating access tokens. LSASS is frequently the target of adversarial efforts to dump credentials for the purposes of privilege escalation and lateral movement. 
  

## Preparation

No Preparation content identified. 

## Risks

No Risks content identified. 

## Guidance

Configure the host-based agent to detect:

-	Process lineage
    -    Instances where LSASS.exe was spawned from a suspicious parent process
    -    LSASS.exe as the parent of any process
-	Process access
    -    LSASS being accessed by a suspicious processes
    -    Filter out legitimate attempts to access LSASS (antivirus, policy enforcement, etc.)
-	File monitoring
    -    The creation of dump files in specific locations 
-	DLL module loads
    -    Commonly abused DLLs to dump LSASS
-	Command-line parameter inspection
    -    Functions used to dump LSASS

## Associated Techniques

- [T1003.001](https://attack.mitre.org/techniques/T1003/001/) – OS Credential Dumping: LSASS Memory
- [T1003.004](https://attack.mitre.org/techniques/T1003/004/) – OS Credential Dumping: LSA Secrets


## Related Countermeasures

- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0018 - Enable Active Directory (AD) Protected Users Security Group
- CM0087 - Disable Credential Caching in the WDigest Authentication Protocol
- CM0088 - Enable Credential Guard
- CM0089 - Enable Local Security Authority (LSA) Protections
- CM0094 - Restrict Domain Admin User Rights and Monitor Protected Users on Workstations

## References

- Detecting and preventing LSASS credential dumping attacks | <https://www.microsoft.com/en-us/security/blog/2022/10/05/detecting-and-preventing-lsass-credential-dumping-attacks/>
- LSASS Memory | <https://redcanary.com/threat-detection-report/techniques/lsass-memory/>
