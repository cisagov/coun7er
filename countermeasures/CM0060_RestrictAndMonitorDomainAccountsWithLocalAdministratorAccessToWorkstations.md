# Restrict and Monitor Domain Accounts with Local Administrator Access to Workstations

* **ID:** CM0060
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; local administrator rights and domain accounts

## Intended Outcome

Restricting and monitoring domain accounts with local administrator access to standard workstations prevents privilege escalation and impedes attempts at credential access and lateral movement. 

## Introduction

When a domain account signs into a standard workstation, those account credentials may be cached. If an adversary compromises a standard workstation, they will be able to retrieve cached credentials belonging to other domain accounts. With these credentials, the adversary will be able to move vertically or horizontally in the network in pursuit of their ultimate objective(s). If the credentials obtained by the adversary belong to a domain account with local administrator access, the adversary will be able to leverage these privileges to persist in the network. 

## Preparation

- Consult operations, documentation and personnel regarding needed actions to be performed as a local admin and device permissions. 

## Risks

No Risks content identified. 

## Guidance

### Monitor

Monitoring activities for domain accounts with local privilege access will involve logging commands and tracking network activity. If an adversary has compromised a domain account with local privileged access to a workstation, their next goal will be to discover more information that will aid in pivoting. They may attempt to steal the SAM and System hive for offline cracking or use mimikatz to dump credentials stored on the host. Monitoring should track for use of the "reg save" command or vssadmin to stage the SAM and SYSTEM file in a folder for exfiltration. Windows Security Event ID 4656 can be used to track calls to the lsass.exe process. 

Attempts could also be made to use other local accounts to connect to a different workstation using PsExec or pass-the-hash tactics. PsExec logs can be tracked by Windows security log 5145 and Windows system log 7045. SMB connections to remote systems could be another indicator of attack. 

### Restrict

Granting domain accounts local privileged access could be permissible in instances where the host is blocked or has restricted access to the internal network. Additionally, no credentials stored or open connections to the host from internal devices should be allowed or else that could enable the adversary to pivot to other hosts with stolen credentials. 

## Associated Techniques

- [T1078](https://attack.mitre.org/techniques/T1078/) - Valid Accounts
- [T1078.002](https://attack.mitre.org/techniques/T1078/002/) - Domain Accounts
- [T1078.003](https://attack.mitre.org/techniques/T1078/003/) - Local Accounts

## Related Countermeasures

- CM0063 - Investigate Suspicious Login Attempts
- CM0084 - Restrict Accounts with Privileged Active Directory (AD) Access from Logging into Endpoints
- CM0085 - Audit Group Policy Objects (GPOs) in Active Directory (AD)
- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts
- CM0102 - Remove Cached Domain Credentials From Workstation
- CM0103 - Deny log on as a batch job
- CM0104 - Enable Interactive Logon and Require Smart Card Policy for Privileged User Accounts
- CM0124 - Disable On-Premise Active Directory (AD) Accounts with Privileged Roles in Entra ID
- CM0126 - Reset Credentials for Local Administrator Accounts Twice
- CM0140 - Configure Windows Audit Policy
- CM0141 - Audit and Restrict PsExec

## References

- GPO Abuse Privilege Escalation to Local Admin | <https://medium.com/@ericwsound/gpo-abuse-privilege-escalation-to-local-admin-cb212a1b4fdc>
- Implementing Least Privilege Administrative Models | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models>
- Appendix H: Securing Local Administrator Accounts and Groups | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/appendix-h--securing-local-administrator-accounts-and-groups>
- Mimikatz detection using Windows Security Event Logs | <https://www.linkedin.com/pulse/mimikatz-detection-using-windows-security-event-logs-samir-b->
- Threat Hunting: How to Detect PsExec | <https://www.praetorian.com/blog/threat-hunting-how-to-detect-psexec/>
- Detecting PsExec lateral movements: 4 artifacts to sniff out intruders | <https://www.hackthebox.com/blog/how-to-detect-psexec-and-lateral-movements>
