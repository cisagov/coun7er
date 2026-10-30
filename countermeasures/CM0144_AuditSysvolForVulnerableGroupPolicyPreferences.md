# Audit SYSVOL for Vulnerable Group Policy Preferences

* **ID:** CM0144
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active
* **Nice Work Role:** Systems Security Analysis (IO-WRL-006)
* **Skills & Knowledge:** Active Directory security assessment; SYSVOL auditing and exposed Group Policy credentials

## Intended Outcome

Auditing vulnerable Group Policy Preferences (GPP) in Active Directory (AD) environments prevents the adversary from achieving defense evasion and persistence via harvesting GPP passwords.

## Introduction

Active directory environments are a prime target for organized threat groups. SYSVOL is a shared folder that exists on the hard disk of a each domain controller within an Active Directory forest, containing functional components such as Group Policy Objects (GPOs) and logon/logoff scripts. For this reason, restoring SYSVOL in AD environments is necessary for a victim to regain authentication, restore functionality, and reestablish synchronization of a corrupted File Replication Service (FRS). Neglecting to restore SYSVOL after compromise invites an attacker to continuously harvest credentials and obscure malicious scripts that enable their persistence within a network.

## Preparation

- Identify content within the SYSVOL and Netlogon shared folders of each domain controller. Note any incongruencies between SYSVOL and Netlogon folders across each domain controller within the AD environment.
- Examine and document the status of the File Replication Service (FRS) within the AD environment, noting inconsistencies and signs of corruption.
- If present, identify existing SYSVOL backups that can be used in authoritative recovery of SYSVOL.
- Conduct system backups before making changes to the SYSVOL tree.

## Risks

- Interacting with a domain's SYSVOL tree and its contents will cause domain controllers to stop service of authentication requests until the SYSVOL folder is shared again.
- Incorrect registry modification performed by following the included guidance may cause malfunction of system startup functions.
- Improper application of this countermeasure can disrupt the synchronization of files/content within the SYSVOL and Netlogon folders.

## Guidance

### Audit SYSVOL Content

Administrators often utilize Group Policy Preferences (GPP) to create domain policies with embedded credentials.
- Search SYSVOL content and delete any GPPs (saved as .XML files) that contain credentials. These will be exposed to an adversary in the event of AD compromise.

To address the aforementioned risks:
- Conduct auditing of the SYSVOL tree outside of common work hours. Schedule maintenance windows to minimize the impact on users and the servicing of authentication requests.
- Backup the registry and maintain detailed records of modifications made to the registry for future reference and troubleshooting.
- The risk of disrupted synchronization can be effectively mitigated by performing regular health checks on the SYSVOL and Netlogon folders to ensure proper functioning. Tools such as the Repadmin command allow regular monitoring and verification of replication status.

## Associated Techniques

- [T1552.006](https://attack.mitre.org/techniques/T1552/006/)- Unsecured Credentials - Group Policy Preferences

## Related Countermeasures

- CM0085 - Audit Group Policy Objects (GPOs) in Active Directory (AD)
- CM0107 - Restore SYSVOL Content in AD Domains
- CM0122 - Restrict SYSVOL Credential Theft

## References

- How to Rebuild the SYSVOL Tree and its Content in a Domain | <https://learn.microsoft.com/en-us/troubleshoot/windows-server/group-policy/rebuild-sysvol-tree-and-content-in-a-domain>

- Securing SYSVOL: Threats, Protection, and Recovery | <https://www.cayosoft.com/sysvol/>

- Repadmin: How to Check Active Directory Replication | <https://activedirectorypro.com/repadmin-how-to-check-active-directory-replication/>
