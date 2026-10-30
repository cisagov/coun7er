# Update Firmware

* **ID:** CM0066
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Systems administration; firmware updates and hardware management

## Intended Outcome

Updating firmware restricts adversary impact, privilege escalation, and lateral movement.

## Introduction

Updating firmware on a device (also called "flashing") overwrites existing firmware with new data to patch security vulnerabilities, fix bugs, and/or replace malicious firmware installed by an adversary. Flashing is essential when corrupted system or component firmware would persist with a reimage (e.g., when a rootkit has corrupted the firmware).

## Preparation

* Understand the potential impact of the update.
* Back up critical data.
* Have a plan for dealing with a corrupted or failed update.

## Risks

* Firmware is often stored in non-volatile memory, so if an update is corrupted or fails, the device may become unusable.
* Operations may be impacted because updating firmware requires system reboots and downtime.

## Guidance

Devices have different update methods, but the steps below apply to most.

* Identify the exact model and version of the device.
* Identify the update required and verify its integrity (firmware updates should come from the device manufacturer).
* Back up the current firmware version.
* Use an uninterruptible power supply (critical).
* Disable antivirus software (re-enable after the update).

Read the documentation provided by the device manufacturer for explicit firmware update steps. Common device access methods are outlined below. 

### Web Interface

If the device has a web interface, launch it and navigate to the firmware settings.

### Custom App or Tool

Some devices offer custom configuration tools. For example, Intel SSD drives use a custom tool called *Intel SSD Toolbox*. Using a custom tool typically involves navigating to the firmware update section and selecting "update."   

## Associated Techniques

* [T0839](https://attack.mitre.org/techniques/T0839/) - Module Firmware
* [T1495](https://attack.mitre.org/techniques/T1495/) - Firmware Corruption

## Related Countermeasures

- CM0067 - Install Critical Software Security Updates
- CM0026 - Reboot Servers
- CM0027 - Rebuild Compromised Host
- CM0081 - Rebuild Citrix NetScaler Appliances
- CM0086 - Reboot Workstations

## References

- How to Update Firmware on Any Device | <https://www.digitalcitizen.life/how-update-firmware-any-device-6-steps/>
- SI-2(5): Automatic Software / Firmware Updates | <https://csf.tools/reference/nist-sp-800-53/r4/si/si-2/si-2-5/>
- Firmware Updates: What They Are and Why They Matter | <https://tech.target.com/blog/firmware-updates>
- Easy Guide to Updating the BIOS on a Computer (Windows) | <https://www.wikihow.com/Update-Your-Computer%27s-BIOS>
