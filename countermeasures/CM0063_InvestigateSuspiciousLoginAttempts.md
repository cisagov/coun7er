# Investigate Suspicious Login Attempts

* **ID:** CM0063
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active
* **Nice Work Role:** Incident Response (PD-WRL-003)
* **Skills & Knowledge:** Incident response; authentication logs and suspicious login analysis

## Intended Outcome

Investigating suspicious login attempts detects adversary credential access and lateral movement.  

## Introduction

Cyber responders should follow best practices regarding monitoring systems for potential breaches. Two areas for consideration are external logins and internal logins. Adversaries may continue to use credential access techniques and/or abuse valid accounts after eradication and recovery efforts. 

### External Login Attempts

External attacks for credential access will be for initial access to the environment. Investigate web application authentication logs, associated IP addresses, and frequency of http requests. An additional area of investigation is remote logins. Network activity should be monitored for signs of brute force, failed login attempts, and origin source. 

### Internal Login Attempts

Internal login attempts may occur when an account in the environment has been compromised and the adversary is looking to move laterally or escalate privileges. Internal logins are suspicious if a logged in user or device is attempting to access accounts or devices not within the scope of their duties or usual activities. 

## Preparation

No Preparation content identified.

## Risks

No Risks content identified.

## Guidance

To investigate suspicious login attempts, consider these 4 "W"s: 

- Who: The user(s) attempting to gain access,
- When: At what times were access attempts occurring and frequency of attempts,
- Where: The associated IP/MAC address the requests are coming from, and
- What: The platform to be accessed (host, network service, web application, etc). 

After applying these 4 "W"s, it may be necessary to lock the account and send a notification to the affected user(s) to reset their secrets. 

### Incident Counter Strategies

To counter suspicious login attempts, it may be necessary to move an account to a protected users security group or an equivalent. Doing so will help protect against credential theft for both devices and users.
Another strategy will be to tune EDR, SIEMs, threat hunting tools, etc., to an ongoing threat actors potential targets. This will be useful in restricting an adversary's actions, determining their goals, and removing them from all affected systems. 

#### Windows Event Logs

Responders should review event logs in case of an incident to check for suspicious activity. These logs can be found in C:\Windows\System32\winevt\Logs\ or in event viewer's Window's Security logs and filtering for event IDs 
* Event ID: 4624 Captures successful logon events and will be an important log to monitor. This log also captures "Logontypes" with types 3, 9, and 10 being most important for suspicious activity. 
    - Logon type 3 refers to users logging in through network shares such as SMB. Adversaries will often use pass-the-hash or a discovered password to get an interactive shell or discover more information using a user's credentials. 
    - Logontype 9 can be suspicious if a regular user uses the "runas" command to perform actions as another user on the network. 
    - Logontype 10 captures remote log-in such as WinRM and RDP which is often used by adversaries that have stolen credentials. 
 * Event ID: 4625 Captures failed logon events and will an another important event to monitor. It will be important to pay attention to the status codes which are in hexadecimal format.
     - 0xC000006F describes a user that attempts to log-in outside of work hours. If the machine is configured to reject logon attempts outside of duty hours, this could be an indicator of attack (IoA).
     - 0xc000015b describes a user that attempts to log-in to a machine not associated with their account. This could be a sign of pivoting attempts by an adversary and should be monitored. 
* Event ID: 4648 Captures logons using explicit credentials such as with the "runas" command. 

##### Enable Auditing

To enable auditing of logon events on a host follow these steps: Open gpmc.msc -> Computer Configuration -> Windows Settings -> Security Settings -> Local Policies -> Audit Policy -> Audit Account Logon Events -> Checkmark Success and Failure. 
The steps are the same for Active directory except for these additional steps: Open gpmc.msc on domain controller -> Select Domain -> right-click domain and click "Create a GPO in this domain, and Link it here" -> Enter a GPO name -> Right click on new GPO and select edit. Then follow the rest of the steps from enabling on a host. 

### Post-Incident Activities

After an incident occurs, compromised accounts should be monitored to verify no backdoor access mechanisms remains. This would include verifying vulnerabilities that were patched have not created another vulnerability that allows access to the same or different accounts. 

## Associated Techniques

- [T1110.001](https://attack.mitre.org/techniques/T1110/001/) - Brute Force: Password Guessing
- [T1110.003](https://attack.mitre.org/techniques/T1110/003/) - Brute Force: Password Spraying
- [T1110.004](https://attack.mitre.org/techniques/T1110/004/) - Brute Force: Credential Stuffing
- [T1078](https://attack.mitre.org/techniques/T1078/) - Valid Accounts

## Related Countermeasures

- CM0018 - Enable Active Directory (AD) Protected Users Security Group
- RM0092 - Disable Compromised or Illegitimate User Accounts (*future*)
- CM0060 - Restrict and Monitor Domain Accounts with Local Administrator Access to Workstations
- CM0064 - Investigate Account Manipulation
- CM0084 - Restrict Accounts with Privileged Active Directory (AD) Access from Logging into Endpoints
- CM0112 - Remove Extraneous and Stale Accounts
- CM0114 - Enable Microsoft Conditional Access Policy Templates
- CM0118 - Revoke Existing Administrator Permissions and Deploy New Administrator Accounts
- CM0121 - Enable Managed Domain Authentication
- CM0123 - Remove Malicious Domain Trust from Tenant
- CM0124 - Disable On-Premise Active Directory (AD) Accounts with Privileged Roles in Entra ID
- CM0125 - Reset and Monitor adminCount Attribute
- CM0126 - Reset Credentials for Local Administrator Accounts Twice
- CM0128 - Reset Credentials for Global and Domain Administrator Accounts Twice
- CM0137 - Remove Global Catalog from Domain Controller
- CM0140 - Configure Windows Audit Policy

## References

- How to investigate user logins | <https://www.cybertriage.com/blog/training/how-to-investigate-user-logins-intro-to-incident-response-triage-2021/>
- Audit logon events | <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/basic-audit-logon-events>
- Windows Security Log Event ID 4624 | <https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=4624>
- Windows Security Log Event ID 4625 | <https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=4625>
- 4624 (S): An account was successfully logged on | <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4624>
- How to See Who Logged Into a Computer | <https://www.howtogeek.com/124313/how-to-see-who-logged-into-a-computer-and-when/>
- Check User Login History in Windows Active Directory | <https://www.lepide.com/how-to/audit-who-logged-into-a-computer-and-when.html>
- Audit logon Events | <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/basic-audit-logon-events>
- How to Detect Pass-The-Hash Attacks | <https://blog.netwrix.com/2021/11/30/how-to-detect-pass-the-hash-attacks/>

