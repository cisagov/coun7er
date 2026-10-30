# Restrict SYSVOL Credential Theft

* **ID:** CM0122
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; SYSVOL permissions and Group Policy credential protection

## Intended Outcome

Restricting access to credentials stored in SYSVOL folders prevents adversaries from stealing or misusing sensitive data embedded in Group Policy Preferences (GPP) thereby limiting their ability to escalate privileges, maintain persistence, and/or perform lateral movement using the acquired credentials.

## Introduction

Active directory environments are a prime target for organized threat groups, as compromising SYSVOL can grant attackers access to sensitive credentials and configurations. The SYSVOL shared folder contains important components such as Group Policy Objects (GPOs), Group Policy Preference (GPP) files, and logon/logoff scripts, which are critical for domain authentication.  

## Preparation

* **Backup SYSVOL:** Ensure a reliable backup of SYSVOL exists before making any changes to prevent unintended disruptions.

## Risks

- Incorrect modification of SYSVOL contents or permissions can lead to disruptions in Group Policy Object (GPO) functionality.
- Failure to audit and remove unsecured credentials can lead to the compromise of privileged accounts.
- Altering SYSVOL or Netlogon folder permissions without proper preparation can lead to disruptions in domain controller services.

## Guidance

### Restrict Credential Exposure in SYSVOL

1. **Identify and Remove Embedded Credentials:**
    
    - Search for `.XML` files in SYSVOL that may contain embedded credentials used in Group Policy Preferences (GPPs).
    - Delete any credentials found in these files, particularly those that use `cpassword` attributes, which store password data in an insecure format.
    - Ensure that administrative accounts and sensitive services do not rely on embedded credentials in GPPs.
2. **Secure SYSVOL Access Permissions:**
    
    - Audit access control lists (ACLs) on SYSVOL and Netlogon shared folders.
    - Restrict access to SYSVOL to only necessary accounts and services, such as domain admins and system services.
    - Review and minimize the number of users with the ability to edit GPOs that affect SYSVOL content.
3. **Utilize Protected Users Security Group:**
    
    - Add privileged accounts to the **Protected Users** security group to prevent these accounts from being cached in memory or stored in SYSVOL with embedded credentials.
    - This group prevents members from using legacy authentication mechanisms, reducing exposure to attacks on SYSVOL or GPP-stored credentials.
4. **Monitor and Audit SYSVOL Changes:**
    
    - Enable logging of changes to SYSVOL directories and monitor for unauthorized modifications.
    - Regularly audit SYSVOL for new GPPs or other policies that might store credentials or introduce other vulnerabilities. Pay particular attention to GPP .xml files.
5. **Implement Privileged Access Management (PAM):**
    
    - Use a Privileged Access Management (PAM) solution to better control access to privileged accounts and sensitive data within SYSVOL.
    - Ensure all privileged access is granted for limited timeframes and with proper monitoring, reducing the chance of credentials being misused.

### Audit SYSVOL Content

* **Continuous Auditing:** Establish a regular cadence for auditing SYSVOL content, focusing on GPP files and any potentially exposed credentials.
* **Remove Obsolete Files:** Delete any unused or obsolete files and GPOs within SYSVOL that could serve as attack vectors.

## Associated Techniques

- [T1484.001](https://attack.mitre.org/techniques/T1484/001/)- Domain or Tenant Policy Modification: Group Policy Modification
- [T1552.006](https://attack.mitre.org/techniques/T1552/006/)- Unsecured Credentials: Group Policy Preferences
-  [T1078](https://attack.mitre.org/techniques/T1078/)- Valid Accounts
-  [T1615](https://attack.mitre.org/techniques/T1615/) - Group Policy Discovery

## Related Countermeasures

- CM0018 - Enable Active Directory (AD) Protected Users Security Group
- CM0069 - Reset Directory Services Restore Mode (DSRM) Account Passwords on Domain Controllers
- CM0102 - Remove Cached Domain Credentials From Workstation
- CM0107 - Restore SYSVOL Content in AD Domains
- CM0144 - Audit SYSVOL for Vulnerable Group Policy Preferences (GPP)

## References

- How to Rebuild the SYSVOL Tree and its Content in a Domain | <https://learn.microsoft.com/en-us/troubleshoot/windows-server/group-policy/rebuild-sysvol-tree-and-content-in-a-domain>

- Securing SYSVOL: Threats, Protection, and Recovery | <https://www.cayosoft.com/sysvol/>

- Privilege Escalation via Group Policy Preferences (GPP) | <https://www.mindpointgroup.com/blog/privilege-escalation-via-group-policy-preferences-gpp>


