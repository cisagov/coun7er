# Remove Cached Domain Credentials From Workstation

* **ID:** CM0102
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Eliminate
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; Active Directory, Group Policy, cached domain credentials and credential removal

## Intended Outcome

Removing cached domain credentials from workstations prevents adversaries from pursuing credential access via password stores and OS credential dumping. 

## Introduction

Caching domain credentials locally allows users to log-in if domain controller(s) are down. Adversaries abuse this functionality by dumping cached credentials using tools such as mimikatz.

## Preparation

- Identify which users, if any, should have credentials cached to maintain business operations or communications. 

## Risks

- Users won't be able to login if authentication servers are down and domain controller authentication is required. 
- Users won't be able to access networked resources. 

## Guidance

### Configure via Group Policy

Open gpedit.msc -> Computer Configuration -> Windows Settings -> Security Settings -> Local Policies -> Security Options -> Double-click Policy Named, "Interactive logon: Number of previous logons to cache (in case domain controller is not available)." -> Select 0 
This will set previous cached credentials to zero. run gpuupdate force

### Configure via Registry

Open regedit.exe -> Navigate to HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\ -> Double-Click "cachedlogonscount" -> Set Value to 0. 
Restart will be required to enforce changes. 

## Associated Techniques

- [T1003.005](https://attack.mitre.org/techniques/T1003/005/) - OS Credential Dumping: Cached Domain Credentials
- [T1550](https://attack.mitre.org/techniques/T1550/) - Use Alternate Authentication Material
- [T1078.002](https://attack.mitre.org/techniques/T1078/002/) - Valid Accounts: Domain Accounts

## Related Countermeasures

- CM0060 - Restrict and Monitor Domain Accounts with Local Administrator Access to Workstations
- CM0122 - Restrict SYSVOL Credential Theft

## References

- How to remove saved domain credentials from workstation | <https://learn.microsoft.com/en-us/answers/questions/1611007/how-to-remove-saved-domain-credentials-from-a-work>
- Mimikatz | <https://redcanary.com/threat-detection-report/threats/mimikatz/>
- Credential Dumping: Domain Cache Credential | <https://www.hackingarticles.in/credential-dumping-domain-cache-credential/>
- Campus Active Directory - Windows Endpoints - Caching Credentials for Remote Users | <https://kb.wisc.edu/iam/page.php?id=114011#:~:text=Caching%20credentials%20is%20a%20feature,no%20domain%20controllers%20are%20available.>
