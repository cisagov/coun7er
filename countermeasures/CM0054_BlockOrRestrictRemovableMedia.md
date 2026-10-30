# Block or Restrict Removable Media

* **ID:** CM0054
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Endpoint administration; removable media and device control

## Intended Outcome

Blocking or restricting removable media blocks adversary initial access and lateral movement using malware located on removable media and blocks adversary exfiltration using removable media.

## Introduction

Removable media bypasses firewalls and intrusion detection systems, making it a prime conduit for malware. Removable media can also be a channel for exfiltrating sensitive data.

## Preparation

- Assess the impact to operations if AutoRun is disabled.
- Assess the impact to operations if removable media is blocked or restricted.
- Define an appropriate removable media policy.

## Risks

No Risks content identified.

## Guidance

Removable media can be disabled or restricted using group policy or the registry. 

### Group Policy

- To block or restrict removable media across the organization at the machine level, create/open a GPO to configure the **Computer Configuration > Policies > Administrative Template > System > Removable Storage Access** node.
- To block or restrict removable media across the organization at the user level, create/open a GPO to configure the **User Configuration > Policies > Administrative Template > System > Removable Storage Access** node.

### Registry

Registry key values can be set to disable AutoRun or to deny access to removable storage. (Create registry keys as needed.)

#### Disable AutoRun

- Set the value of the **HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\NoDriveTypeAutoRun** key to **FF** to disable AutoRun on all drives (different bits and bitmask constants can be used to disable AutoRun for particular drive types).

#### Disable Access to Removable Storage Devices

- Set the value of the **HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\RemovableStorageDevices\Deny_All** key to **1** to disable access to removable storage devices.

## Associated Techniques       

- [T1091](https://attack.mitre.org/techniques/T1091/) - Replication Through Removable Media
- [T1025](https://attack.mitre.org/techniques/T1025/) - Data from Removable Media

## Related Countermeasures

- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules

## References

- Enabling and Disabling AutoRun | <https://learn.microsoft.com/en-us/windows/win32/shell/autoplay-reg>
- How to disable removable media access with Group Policy | <https://www.techtarget.com/searchSecurity/tutorial/How-to-disable-removable-media-access-with-Group-Policy>
- How to Disable AutoRun and AutoPlay for External Devices | <https://www.lifewire.com/disable-autorun-on-a-pc-153344>
- How to disable access to removable storage devices on Windows 10 | <https://www.windowscentral.com/how-disable-access-removable-storage-devices-windows-10>
