# Rebuild Compromised Host

* **ID:** CM0027
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Systems administration; host rebuilding, imaging, and baseline configuration

## Intended Outcome

Rebuilding a compromised host terminates adversary persistence, command
and control, and execution using the compromised host.

## Introduction

Reimaging involves restoring the systems to their original state by reverting to a clean image which can help remove malware and associated artifacts. Rebuilding systems can be resource intensive and involves restoring systems to their original state by comprehensively rebuilding different aspects or parts of a system to a previous desired point (files, programs, settings, etc.).

## Preparation

- Determine the scope and impact of the incident to identify which systems need to be reimaged/rebuilt. 
    - It is imperative to accurately determine the scope of a compromise to eliminate the ability for adversaries to maintain access past an initial response to a compromise. Without this, the potential for a drawn out and reactionary (as opposed to proactive) engagement of repeatedly removing an adversary's access increases.
- Identify and prepare clean Operating System (OS) images, application installations, and files for restoration and recovery efforts.
    - Confirm the integrity of any clean images/software prior to use, to avoid the risk of re-infection.
    - Checks must verify that no infection or malicious files are present.
- Check for available backups to minimize data loss. 
    - If a backup of critical data on an impacted system does not existed, performing one should be done with extreme caution. Attempts at preserving any data on an impacted system runs the risk of preserving compromised data, allowing persistence of adversaries, and carrying over any general compromises to the reimaged system. 
    - Confirm backups are free of compromise. 
    - Any data that is attempted to be backed up should be examined extremely carefully and thoroughly prior to use.

## Risks

- This countermeasure may have impacts that disrupt operations, including
    - Data loss: Data entered since the last clean backup may be lost.
    - Business disruption: Downtime during business hours may disrupt business.
- Errors can render this countermeasure ineffective.

## Guidance

The specifics for rebuilding or performing a reimage of impacted systems will vary with each scenario. The following steps are considered best practices but do not represent an exhaustive list, and a security professional should be consulted to create and execute a comprehensive Rebuilding/Reimaging plan.

- Verify the integrity of selected clean images/software
    - Images/software used in rebuilding/reimaging efforts must be clean and free from infection/issues.
- Securely wipe affected systems
    - Identify an appropriate solution to securely wipe affected systems. Many specialized software solutions are available that are designed for this.
- Reinstall resources for affected systems (software, operating systems, etc.)
    - Utilize a verified clean image/copy to reinstall the operating system/software.
    - Many systems provide specific guidance on how to properly reinstall operating systems or software and appropriate documentation should be consulted to ensure the best results.
- Deploy any necessary patches

## Associated Techniques

- [T1547](https://attack.mitre.org/techniques/T1547/) - Boot or Logon Autostart Execution
- [T1037](https://attack.mitre.org/techniques/T1037/) - Boot or Logon Initialization Scripts
- [T1176](https://attack.mitre.org/techniques/T1176/) - Browser Extensions
- [T1554](https://attack.mitre.org/techniques/T1554/) - Compromise Host Software Binary
- [T1136.001](https://attack.mitre.org/techniques/T1136/001) - Create Account: Local Account
- [T1543](https://attack.mitre.org/techniques/T1543/) - Create or Modify System Process
- [T1546](https://attack.mitre.org/techniques/T1546/) - Event Triggered Execution
- [T1574](https://attack.mitre.org/techniques/T1574/) - Hijack Execution Flow
- [T1137](https://attack.mitre.org/techniques/T1137/) - Office Application Startup
- [T1653](https://attack.mitre.org/techniques/T1653/) - Power Settings
- [T1053](https://attack.mitre.org/techniques/T1053/) - Scheduled Task/Job
- [T1078.003](https://attack.mitre.org/techniques/T1078/003) - Valid Accounts: Local Accounts
- [T1092](https://attack.mitre.org/techniques/T1092/) - Communication Through Removable Media
- [T1659](https://attack.mitre.org/techniques/T1659/) - Content Injection
- [T1568](https://attack.mitre.org/techniques/T1568/) - Dynamic Resolution
- [T1105](https://attack.mitre.org/techniques/T1105/) - Ingress Tool Transfer
- [T1203](https://attack.mitre.org/techniques/T1203/) - Exploitation for Client Execution
- [T1569](https://attack.mitre.org/techniques/T1569/) - System Services
- [T1204](https://attack.mitre.org/techniques/T1204/) - User Execution

## Related Countermeasures

- RM0137 - Implement IT Disaster Recovery Plans (*future*)
- CM0059 - Configure Tactical Privileged Access Workstation
- CM0065 - Isolate Endpoints from Network
- CM0066 - Update Firmware
- CM0067 - Install Critical Software Security Updates
- CM0081 - Rebuild Citrix NetScaler Appliances
- RM0449 - Restore Host to Backup (*future*)

## References

- Computer Security Incident Handling Guide | <https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf>
- Cybersecurity Incident & Vulnerability Response Playbooks | <https://www.cisa.gov/sites/default/files/2024-08/Federal_Government_Cybersecurity_Incident_and_Vulnerability_Response_Playbooks_508C.pdf>
