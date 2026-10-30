# Eliminate Hidden Registry Keys

* **ID:** CM0129
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Eliminate
* **Status:** Active
* **Nice Work Role:** Incident Response (PD-WRL-003)
* **Skills & Knowledge:** Windows incident response; hidden registry key analysis and persistence removal

## Intended Outcome

Eliminating hidden registry keys will eliminate adversarial attempts to evade defenses and achieve persistence by hiding malicious code in the registry.  

## Introduction

Adversaries commonly attempt to persist in the Windows registry by creating or modifying registry keys. Persistence via registry keys enables an adversary to execute code and persist through system reboots. These persistence techniques typically attempt to masquerade as legitimate registry keys. 

This countermeasure is specifically intended to address persistence via hidden registry keys. Hidden keys are prepended with a NULL byte (\x00) to impede detection and response. Neither regedit.exe nor reg.exe can view registry keys containing NULL bytes.    

## Preparation

-	Ensure an effective backup strategy.

## Risks

The Windows registry is a critical database that stores configuration settings for the operating system. Eliminating registry keys can introduce risk and in some instances, render a system unusable. Take care to analyze the registry key in question to minimize the risk of operational disruption. To further offset this risk, ensure a recent backup is available before eliminating registry keys.   

## Guidance

-	Consider implementing detection logic and continually monitor attempts to modify the registry. At a minimum, consider the following registry paths:

    - `HKEY_USERS\*\Software\Microsoft\Windows\CurrentVersion\Run`
    - `HKU\*\Software\Microsoft\Windows\CurrentVersion\Run`
    - `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`
    - `HKLM\Software\WOW6432Node\Microsoft\Windows\CurrentVersion\Run`
    - `HKEY_USERS\*\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run`
    - `HKU\*\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run`
    - `\REGISTRY\MACHINE\Software\Microsoft\Windows\CurrentVersion\Run`
    - `\REGISTRY\USER\*\Software\Microsoft\Windows\CurrentVersion\Run`
    - `\REGISTRY\MACHINE\Software\Microsoft\Windows\CurrentVersion\Run`
    - `\REGISTRY\MACHINE\Software\WOW6432Node\Microsoft\Windows\CurrentVersion\Run`
    - `\REGISTRY\MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run`
    - `\REGISTRY\MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run`

-	If malicious activity in the registry is suspected, attempt to assess the run keys. Attempts to hide a registry key with a preceding NULL byte will result in the following error: "Cannot display: Error reading the value’s contents."
-	Identify and remove the registry key:
    - Microsoft provides a command-line [utility](https://learn.microsoft.com/en-us/sysinternals/downloads/regdelnull) called 'RegDelNull' in the Sysinternals suite that enables defender to search for and delete registry keys that contain embedded NULL characters. 
        - Utilize RegDelNull to delete null-embedded keys: `regdelnull <Registry_Key> [-s]`.
    -   Huntress Labs provides two [utilities](https://support.huntress.io/hc/article_attachments/4404012538515) for identifying and removing hidden registry keys.
        - GetRegKey displays all values in a registry key: `GetRegKey.exe <Registry_Key>`.
        - DelRegValue deletes a single registry value: `DelRegValue <Registry_Key>`.

## Associated Techniques

-	[T1547.001](https://attack.mitre.org/techniques/T1547/001/) – Boot or Logon Autostart Execution: Registry Run Keys/Startup Folders
-	[T1112](https://attack.mitre.org/techniques/T1112/) – Modify Registry

## Related Countermeasures

No Related Countermeasures content identified.

## References

-	Hiding Registry Keys with PSReflect | <https://posts.specterops.io/hiding-registry-keys-with-psreflect-b18ec5ac8353>
-	How to Evade Detection: Hiding in the Registry | <https://tripwire.com/state-of-security/evade-detection-hiding-registry>
-	Manually Remediate Kovter Hidden Registry Values | <https://support.huntress.io/hc/en-us/articles/4404012536979-Manually-Remediate-Kovter-Hidden-Registry-Values>
- RegDelNullv1.11 | <https://learn.microsoft.com/en-us/sysinternals/downloads/regdelnull>
- Manually Remediate Kovter Hidden Registry Values | <https://support.huntress.io/hc/en-us/articles/440401-Manually-Remediate-Kovter-Hidden-Registry-Values>
- Huntress Labs Utilities | <https://support.huntress.io/hc/article_attachments/4404012538515>
