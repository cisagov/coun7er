# Identify and Terminate Suspicious Processes

* **ID:** CM0023
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine, Eliminate
* **Status:** Active
* **Nice Work Role:** Incident Response (PD-WRL-003)
* **Skills & Knowledge:** Incident response; process analysis and termination

## Intended Outcome

Identifying and terminating suspicious processes terminates adversary execution of malware.

## Introduction

Suspicious or anomalous processes include unrecognized process names, especially names that imitate a legitimate or benign system processes or appear randomly generated. Characteristics of suspicious processes include: 

- Processes with no icon, version information, description, or company name. 
- Processes that are unsigned, especially a process with a company name like Microsoft Corporate that is also unsigned. 
- Processes hosted by service host executables and other Windows utilities without a valid parent-child relationship with the principal Windows processes. 
- Processes that have used compression as compressed code may indicate obfuscation or encryption. Processes explorer highlights these in purple.

## Preparation

- Disconnect the system from the network.
- Ensure that sanitized and approved backups are available.
- Create forensic image of the machine.

## Risks

- This countermeasure can break legitimate functionality.
- Terminating legitimate processes may impact system or application operation.

## Guidance

### Investigate Suspicious or Anomalous Processes

Examine how the process interacts with the Registry, the file system, and on the network. Seek to answer the following questions:

- How is the process launched?
- What files is it manipulating?
- Is the process restored on reboot? If so, how?
- Is system privilege inhibiting the ability to terminate or access the process?
- Is the process associated with any ports? Is the process communicating with any domains or IP subnets?
- If the process is legitimate but acting in an unusual way, could the process be injected with malicious code? What would the impact of suspending or terminating a hijacked legitimate process?

### Terminate Suspicious or Anomalous Process

- Contain the affected system.  Consider the impact of disconnecting the affected system from the network.  If viable, disconnect the affected system from the network.  
- Collect information about the affected system and suspicious process.  Take a system image and/or dump process information from memory prior to terminating the process and repairing the compromised system. 
- Terminate the suspended process only if doing so is necessary to begin the remediation process.  Remove relevant persistence mechanisms (auto-start locations, scheduled tasks, registry keys, etc.) This may require reverting the affected system to a known secure baseline and reinstalling the operating system. 
- Reboot the system and monitor for the existence of the process and anomalous network activity.  If suspicious activity persists, consider reinstalling the operating system and applications with an approved secure baseline.  Continued persistence may indicate firmware or software supply chain compromise.  
- Readmit the system into the production environment only after hardening and continuing to closely monitor.    

## Associated Techniques

- [T1543](https://attack.mitre.org/techniques/T1543) - Create or Modify System Processes
- [T1055](https://attack.mitre.org/techniques/T1055) - Process Injection

## Related Countermeasures

- CM0002 - Enable Email Attachment Filtering and Message Authentication
- CM0049 - Disable or Monitor Executables with SUID and SGID Bits Set
- CM0024 - Disconnect Internet-facing Boundary Controllers
- CM0065 - Isolate Endpoints from Network
- CM0030 - Remove Known Malware
- CM0034 - Identify and Block Suspicious Hosts
- CM0123 - Remove Malicious Domain Trust from Tenant
- CM0135 - Rebuild Group Managed Service Accounts (gMSA)
- CM0147 - Remove Suspicious Scheduled Tasks

## References

No References content identified. 

