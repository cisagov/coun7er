# Install Critical Software Security Updates

* **ID:** CM0067
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Patch management; software updates and change control

## Intended Outcome

Installing critical software security updates restricts adversary execution, persistence, credential access, lateral movement, collection, privilege escalation, and initial access techniques that exploit unpatched and vulnerable software.

## Introduction

A security update is a patch or modification to software or systems released by developers to correct vulnerabilities. A critical security update is often announced by a vendor to correct a zero-day/0-day vulnerability. Organizations that experience an incident involving exploitation of one or more vulnerabilities by an adversary should ensure that security updates are applied as soon as possible. 

## Preparation

- Acquire and review documentation and guidance provided by the manufacturer and follow recommended steps when applying and configuring a security update.
- Ensure periodic backups are taken and restore points are available. It may be necessary to create a new backup or create a restore point before installing the update.
- Identify the software, operating system, or appliance that critical security updates will be applied to. An existing inventory of software and assets may be needed to ensure emergency software updates are installed to all relevant systems and software appliances. 

- Understand how the security update may affect the functionality of the organization's environment. This may involve testing the update before rolling it out to a production environment or using canary assets to confirm the patch is neither corrupted or breaks the software.
- Verify the authenticity and integrity of the security update, ensuring it came directly from the manufacturer.
- Prioritize the installation of critical software security updates based on risk, likelihood, and severity if multiple updates are needed.

## Risks

- Installing a critical security update may affect the functionality of the product or service.
- Patching a vulnerability by applying an update alone is insufficient to fully remediate an incident, as a threat actor may have already established persistence in the environment or gained access to sensitive information or credentials that may be used to facilitate follow-on actions.
- Emergency software updates may inadvertently align with the adversary's goals if the software's supply chain is compromised. In this is the case, organization's should be prepared to roll back said patch to a secure baseline. 
- Inability to update certain codebases like Java due to backwards compatibility issues.

## Guidance

Software, systems, and appliances may have different steps to correct vulnerabilities. The exact process of implementing a critical software security update will depend on the instructions advised by the manufacturer, characteristics of the environment the software resides in, and the circumstances (e.g. urgency, environment scale) responders are operating in. Further detailed guidance concerning enterprise patch management may be found within the most recent revision of the National Institute of Standards and Technology (NIST) Special Publication 800-40.

### Apply Critical Security Update

Organizations should apply the emergency update as soon as possible. In some cases where a security update will disrupt operations, it may be necessary to schedule the update, notifying the affected parties in advance if appropriate. 

Prioritized installing security updates when adversaries are known to have exploited current software versions.

Verify the software patch has been installed successfully.

### Monitor Software and Environment

After applying the update, monitor the software environment to identify unintended side effects, as well as instances where the update was "rolled-back" by the adversary, whether unintentionally or intentionally. 

### Document Patching

Document the patch process and track progress. Validate through vulnerability scanning.

### Implement Compensating Controls

If a security update is not yet available or the software has reached end-of-life, organizations should implement compensating controls until a fix is made available or a replacement solution can be acquired. This may include taking the affected software or appliance offline. 

### Enable Automatic Updates

Enable automatic updates, if not already configured. 

## Associated Techniques

Installing critical software updates and patching system vulnerabilities may counter subsequent execution of multiple ATT&CK techniques. These may include:

- [T1548](https://attack.mitre.org/techniques/T1548/) - Abuse Elevation Control Mechanism
- [T1548.002](https://attack.mitre.org/techniques/T1548/002/) -	Abuse Elevation Control Mechanism: Bypass User Account Control
- [T1548.006](https://attack.mitre.org/techniques/T1548/006/) - Abuse Elevation Control Mechanism: TCC Manipulation
- [T1176](https://attack.mitre.org/techniques/T1176/) - Browser Extensions
- [T1555](https://attack.mitre.org/techniques/T1555/) - Credentials from Password Stores
- [T1555.005](https://attack.mitre.org/techniques/T1555/005/) - Credentials from Password Stores: Password Managers
- [T1602](https://attack.mitre.org/techniques/T1602/) - Data from Configuration Repository
- [T1602.001](https://attack.mitre.org/techniques/T1602/001/) - Data from Configuration Repository: SNMP (MIB Dump)
- [T1602.002](https://attack.mitre.org/techniques/T1602/002/) - Data from Configuration Repository: Network Device Configuration Dump
- [T1189](https://attack.mitre.org/techniques/T1189/) - Drive-by Compromise
- [T1546](https://attack.mitre.org/techniques/T1546/) - Event Triggered Execution
- [T1546.010](https://attack.mitre.org/techniques/T1546/010/) - Event Triggered Execution: AppInit DLLs
- [T1546.011](https://attack.mitre.org/techniques/T1546/011/) - Event Triggered Execution: Application Shimming
- [T1190](https://attack.mitre.org/techniques/T1190/) - Exploit Public-Facing Application
- [T1212](https://attack.mitre.org/techniques/T1212/) -  Exploitation for Credential Access
- [T1211](https://attack.mitre.org/techniques/T1211/) - Exploitation for Defense Evasion
- [T1068](https://attack.mitre.org/techniques/T1068/) -  Exploitation for Privilege Escalation 
- [T1210](https://attack.mitre.org/techniques/T1210/) - Exploitation of Remote Services
- [T1574](https://attack.mitre.org/techniques/T1574/) -  Hijack Execution Flow  
- [T1574.002](https://attack.mitre.org/techniques/T1574/002/) - Hijack Execution Flow: DLL Side-Loading
- [T1137](https://attack.mitre.org/techniques/T1137/) - Office Application Startup
- [T1072](https://attack.mitre.org/techniques/T1072/) -  Software Deployment Tools 
- [T1195](https://attack.mitre.org/techniques/T1195/) - Supply Chain Compromise
- [T1195.001](https://attack.mitre.org/techniques/T1195/001/) - Supply Chain Compromise: Compromise Software Dependencies and Development Tools
- [T1195.002](https://attack.mitre.org/techniques/T1195/002/) - Supply Chain Compromise: Compromise Software Supply Chain
- [T1552](https://attack.mitre.org/techniques/T1552/) - Unsecured Credentials
- [T1552.006](https://attack.mitre.org/techniques/T1552/006/) -	Unsecured Credentials: Group Policy Preferences
- [T1550.002](https://attack.mitre.org/techniques/T1550/002/) - Use Alternate Authentication Material: Pass the Hash

## Related Countermeasures

- CM0049 - Disable or Monitor Executables with SUID and SGID Bits Set
- CM0066 - Update Firmware
- CM0026 - Reboot Servers
- CM0027 - Rebuild Compromised Host
- CM0086 - Reboot Workstations
- CM0106 - Eliminate Web Shells
- CM0108 - Detect Rogue Virtual Machines
- CM0138 - Audit and Remove Malicious Udev Rules

## References

- NIST SP 800-40 Rev. 4: Guide to Enterprise Patch Management Planning: Preventive Maintenance for Technology | <https://csrc.nist.gov/pubs/sp/800/40/r4/final>
- Quest: Developing a zero-day patching strategy | <https://csrc.nist.gov/pubs/sp/800/40/r4/final>
- CISA Insights: Remediate Vulnerabilities for Internet-Accessible Systems | <https://csrc.nist.gov/pubs/sp/800/40/r4/final>
