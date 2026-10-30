# Prevent and Detect Attempts to Copy NT Directory Services Database Information Table (NTDS.dit)

* **ID:** CM0096
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Defensive Cybersecurity (PD-WRL-001)
* **Skills & Knowledge:** Active Directory security; NTDS.dit protection and credential-theft detection

## Intended Outcome

Preventing and detecting attempts to copy the NT Directory Services Database Information Table (NTDS.dit) will prevent and alert defenders to attempts to duplicate the file for exfiltration. 

## Introduction

NTDS.dit is a database file hosted on Domain Controllers (DC).  The file stores information on all existing objects in the domain, to include users, service accounts, group memberships, password hashes, and other sensitive details. NTDS.dit cannot be copied directly, a volume shadow copy must be created.  This copy is typically archived, staged, and exfiltrated.  Once exfiltrated, information can be extracted and used to achieve credential access, lateral movement, and persistence.  Preventing attempts to copy the NTDS file is an effort to limit the extent of an adversary's advance and is therefore, a preemptive measure to contain the adversary.  

## Preparation

- Identify options for remotely executing commands on DC e.g. WMI, PowerShell/WMI, WinRM, PowerShell Remoting.
- Verify that appropriate protection and detection measures are in place to mitigate the risks of remote command execution on domain controllers (see related countermeasures).

## Risks

- No Risks content identified. 

## Guidance

### Prevent

-	Minimize the number of accounts that can log on to DC.
-	Restrict access to the NTDS.dit file using Access Control Lists (ACLs).  
-	If possible, enable BitLocker to encrypt the operating system on the DC.
-	Audit and monitor access to the NTDS.dit file. 
-	Ensure effective endpoint logging, aggregation, and monitoring for all workstations and servers, to include DC.  

### Detect

Monitor endpoint protection and event logs for:

-	Process creation (Event ID 4688)
    - Vssadmin.exe (remote)
    - Ntdsutil.exe (local)
    - Esentutl.exe (local)

-	Command-line parameter inspection (PowerShell, Wmic, Vssadmin)
    - Monitor endpoints for tools that can be used to remotely execute commands on DC and the following command line parameters:
        - Wmic.exe `process call create /c vssadmin create shadow`
        - PowerShell.exe `win32_shadowcopy`
        - Vssadmin.exe `create shadow`
        - `Copy \Windows\NTDS\NTDS.dit`
    - Monitor the DC for tools that can be used locally to copy NTDS.dit
        - Ntdsutil.exe `create full`
        - Esentutl.exe `/y /vss`

The various tools used to create volume shadow copies all rely on the Volume Snapshot Service.  For a higher fidelity detection:

-	Monitor shadow copy creation `event ID 8222` 
-	Consider correlating this event to several requests in short succession to create a handle to the Security Account Manager (SAM) `event ID 4661`.  This activity is typically indicative of attempts to generate a shadow copy of C:\.  

## Associated Techniques

- [T1003.003](https://attack.mitre.org/techniques/T1003/003) - OS Credential Dumping: NTDS

## Related Countermeasures

-	CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
-	CM0013 – Block Windows Management Instrumentation (WMI) Service
-	CM0018 - Enable Active Directory (AD) Protected Users Security Group
-	CM0019 – Disable or Restrict Windows Remote Management (WinRM) Protocol
-	CM0031 – Monitor PowerShell Program
-	CM0039 – Monitor Windows Management Instrumentation (WMI) Events
-	CM0088 - Enable Credential Guard
-	CM0089 - Enable Local Security Authority (LSA) Protections

## References

- Forensic Traces of Exporting NTDS | <https://medium.com/@alsaifmay/forensic-traces-of-exporting-ndts-3ee52fcafad3>
- Part 2 Sensor Mapping – Reverse Engineering NTDS | <https://detect.fyi/part-2-sensor-mapping-reverse-engineering-ntds-a73bde69031e>
- Domain of Thrones: Part I | <https://securityboulevard.com/2023/10/domain-of-thrones-part-i/>
- How Attackers Dump Active Directory Database Credentials | <https://adsecurity.org/?p=2398>
