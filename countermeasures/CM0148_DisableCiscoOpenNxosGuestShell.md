# Disable Cisco Open NX-OS Guest Shell

* **ID:** CM0148
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active

## Intended Outcome

Disabling the Cisco NX-OS Guest Shell prevents adversaries from abusing the guest shell to maintain persistent access to a compromised environment.

## Introduction

The Cisco NX-OS Guest Shell is a Linux-based container that is pre-installed on Cisco switches. The Guest Shell enables network admins to access the network over Linux network interfaces, the switch’s boot flash, volatile tmpfs, CLI, host file system, and NX-API REST. Network admins can use the NX-OS Guest Shell to install and run python scripts and 32/64-bit Linux applications.

Adversaries have abused the NX-OS guest shell to achieve initial access and persist in compromised environments. The intent of this countermeasure is to present options for disabling the NX-OS guest shell and revoke persistent access enabled by its abuse.

## Preparation

- Guest shell is supported on Cisco Nexus Series 3000 and 9000 (starting with the 7.0(3)F3(1) release).
- The guest shell is enabled by default on this series of switches.
- Only Network Administrators can access the guest shell by default. As such, you will require Network Administrator privileges to implement this countermeasure.
- If an adversary has accessed the guest shell, identify the account and/or credentials used to authenticate. To contain the adversary, identify where these credentials have been used and revoke them.
- Backup the running configuration before making changes or reloading a new image.
- Ensure the new NX-OS image is compatible with the switch.

## Risks

- Guest shell can be used to build applications, automate tasks, troubleshoot, and conduct logging and tracing. If the organization is depended on the functionality afforded by the guest shell, disabling it can result in operational disruption. To minimize the likelihood of this risk, consider the extent to which the organization employs the guest shell and identify alternative options for accomplishing the same tasks.

## Guidance

Reload a Cisco NX-OS image that does not provide guest shell support
- Log in to the switch (SSH or console).
- Issue the `run guestshell` command to access the guest shell.
- Remove the guest shell: `guestshell destroy`.
- Check the current configuration: `show running-config`.
- Copy the new NX-OS image to the switch’s boot flash memory: `copy tftp://[server-ip]/[image-filename] flash:`.
- Verify the image: `show file bootflash:`.
- Reload the new NX-OS image: `fast-reload [save-config] [nx-bootflash:[image-filename]]`.
- Monitor the reload for issues with the process.

## Associated Techniques

- [T1133](https://attack.mitre.org/techniques/T1133/) – External Remote Services
- [T1610](https://attack.mitre.org/techniques/T1610/) – Deploy Container
- [T1190](https://attack.mitre.org/techniques/T1190/) – Exploit Public-Facing Application
- [T1078.002](https://attack.mitre.org/techniques/T1078/002/) – Valid Accounts: Domain Accounts

## Related Countermeasures

- CM0107 - Restore SYSVOL Content in AD Domains
- CM0149 - Temporarily Disable Cisco Open NX-OS Guest Shell

## References

- Cisco Nexus 9000 Series NX-OS Programmability Guide, Release 7.x | <https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/7-x/programmability/guide/b_Cisco_Nexus_9000_Series_NX-OS_Programmability_Guide_7x/Guest_Shell.html>
