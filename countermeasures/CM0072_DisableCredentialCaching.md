# Disable Credential Caching 

* **ID:** CM0072 
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable 
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; credential caching and authentication settings

## Intended Outcome  

Disabling credential caching prevents adversaries from accessing credentials stored in the domain cache. 

## Introduction 

If a device is unable to reach a domain controller for interactive logon (such as due to network connectivity issues), it may access domain cached credentials (DCC2) locally on a Windows device to facilitate authentication. Adversaries that retrieve DCC2 hashes may be able to brute-force and extract plaintext passwords used for logon on the machine by using cracking tools. This is especially damaging if the plaintext passwords retrieved are used by administrators. 

## Preparation 

No Preparation content identified. 

## Risks 

- Users, including administrators, will not be able to log in to systems with their password if credential caching is disabled and the endpoints are unable to reach back to the domain controller for authentication. 

## Guidance 

### Disable Credential Caching using Group Policy 

The setting “Interactive logon: Number of previous logons to cache (in case domain controller is not available)” refers to a Group policy option that determines how many previously successful interactive logons are stored locally before older logons are purged from the cache. By default, the policy value that controls credential caching is located in Computer Configuration >> Windows Settings >> Security Settings >> Local Policies >> Security Options. To disable credential caching, system administrators should set this policy value to 0. 

### Disable Credential Caching using Registry 

Alternatively, to disable credential caching manually in the registry, the CachedLogonsCount registry key must be changed on each relevant machine in the domain. This registry value can be found at HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\WinLogon\cachedlogonscountvalue. It is generally recommended to modify credential caching by configuring the appropriate Group Policy and to ensure any changes to a systems registry occur only after the registry is backed up. 

### Reduce Number of Credentials Stored in Cache  

While the criticality of a domain-joined system may vary depending on the sensitivity of the data contained therein, it is often recommended that systems keep the number of previous logons to cache (in case domain controller is not available) to the default of 10 or fewer. CIS Critical Security Controls goes further, suggesting it be set to 4 or fewer. In the immediate aftermath of an incident, it may be prudent to reduce the number of cached credentials to a value no less than 2. This will ensure users are able to log on if they are unable to reach the domain controller even if an IT administrator has logged in to the system. 

## Associated Techniques 

- [T1003.005](https://attack.mitre.org/techniques/T1003/005/) - OS Credential Dumping: Cached Domain Credentials 

## Related Countermeasures 

- CM0018 - Enable Active Directory (AD) Protected Users Security Group 

## References 

- Interactive logon: Number of previous logons to cache (in case domain controller is not available) | <https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/interactive-logon-number-of-previous-logons-to-cache-in-case-domain-controller-is-not-available>
- Caching of logon credentials must be limited | <https://www.stigviewer.com/stig/windows_10/2020-06-15/finding/V-63687>
- 2.3.7.7 (L2) Ensure 'Interactive logon: Number of previous logons to cache (in case domain controller is not available)' is set to '4 or fewer logon(s)' | <https://www.tenable.com/audits/items/CIS_Microsoft_Windows_11_Enterprise_v3.0.0_L2.audit:0759bcc846c2f767d9a84dd0c00d8050>
