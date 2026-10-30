# Detect Attempts to Delete Shadow Copies

* **ID:** CM0097
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active
* **Nice Work Role:** Defensive Cybersecurity (PD-WRL-001)
* **Skills & Knowledge:** Windows security monitoring; shadow copies and ransomware behavior

## Intended Outcome

Detecting the deletion of shadow copies will alert defenders to an adversary attempting to destroy volume backups.     

## Introduction

Shadow copies are a form of Windows data backup.  Ransomware actors commonly attempt to delete shadow copies before deploying ransomware to inhibit system recovery and make victim data inaccessible until a ransom is paid.  This is typically accomplished using native Windows binaries (Vssadmin.exe, PowerShell.exe, and WMIC.exe).  

## Preparation

- Ensure systems are frequently performing backups.  Backups should be securely stored on a separate device that is inaccessible from the network.  
- Ensure comprehensive endpoint protection and response.  Logs should be aggregated and processed in a centralized location to enable analysis and response efforts.  

## Risks

- No Risks content identified. 

## Guidance

-	Monitor endpoint protection and event logs for process creation `Event ID 4688`
    - Vssadmin.exe
-	Monitor endpoints for the following command line parameters:
    - Vssadmin.exe `Delete Shadows`
    - Vssadmin.exe `resize shadowstorage`
    - Wmic.exe `shadowcopy delete`
    - PowerShell.exe `win32_shadowcopy`
    - `delete shadows`
    - `list shadows`

## Associated Techniques

- [T1490](https://attack.mitre.org/techniques/T1490/) – Inhibit System Recovery
- [T1486](https://attack.mitre.org/techniques/T1486/) - Data Encrypted for Impact

## Related Countermeasures

- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0045 - Disable or Monitor Execution of WMIC.exe
- CM0013 - Block Windows Management Instrumentation (WMI) Service
- CM0031 - Monitor PowerShell Program
- CM0039 - Monitor Windows Management Instrumentation (WMI) Events

## References

- Volume Shadow Copy Service | <https://learn.microsoft.com/en-us/windows-server/storage/file-server/volume-shadow-copy-service>
- How Can I Protect Against Ransomware? | <https://www.cisa.gov/stopransomware/how-can-i-protect-against-ransomware>
- CAR-2021-01-009: Detecting Shadow Copy Deletion or Resize | <https://car.mitre.org/analytics/CAR-2021-01-009/>
- Its All Fun and Games Until Ransomware Deletes the Shadow Copies | <https://redcanary.com/blog/threat-detection/its-all-fun-and-games-until-ransomware-deletes-the-shadow-copies/>
