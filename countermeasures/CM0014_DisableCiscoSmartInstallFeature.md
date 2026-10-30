# Disable Cisco Smart Install Feature

* **ID:** CM0014
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Network Operations (IO-WRL-004)
* **Skills & Knowledge:** Cisco network administration; device hardening and Smart Install

## Intended Outcome

Disabling the Cisco Smart Install feature blocks adversary collection of
network configuration files and execution of arbitrary commands.

## Introduction

The Cisco Smart Install Client is a legacy utility designed to allow
no-touch installation of new Cisco equipment, specifically Cisco
switches. Adversaries may abuse this feature to access, copy, or poison
a client's startup-config file or maliciously modify the Cisco IOS to
enable persistence.

## Preparation

- Upgrade Cisco devices to latest possible Internetworking Operating System (IOS) release.

## Risks

- This countermeasure increases the risk of breaking legitimate functionality.
- Switch software updates and configuration changes must be performed manually when Smart Install is disabled. 
    - Manual operations increase the likelihood of errors that could cause network inaccessibility or downtime.
    - Manual operations are more labor-intensive than over-the-network operations.
- This countermeasure may result in excess resource use.

## Guidance

- Determine whether Smart Install is Enabled
- Investigate Evidence of Smart Install Abuse
    - Analyze traffic and device logs to determine whether unauthorized changes were made to startup-config, Cisco IOS, or the tftp-server. 
    - Organizations should preserve a copy of the tampered configuration file or any additional evidence of abuse during the incident response investigation.
- Isolate and Restore Affected Device(s)
    - If network administrators determine a network device was abused, they should isolate the affected device from the network.
    - Before bringing the device back online, teams should reverse changes made by the adversary by restoring the device's image and configuration to a secure state. This process may include the use of a known good configuration backup, performing a factory reset of the device, and ensuring the latest vendor patches are applied.
- Restrict and/or Disable Smart Install
    - Network administrators should verify Smart Install is using TCP port 4786 before then blocking it.
    - If Smart Install cannot be disabled, organizations should implement interface access control lists (ACLs) and Control Plane Policing (CoPP) to drop traffic.
- Harden
    - System administrators should ensure all patches for known security vulnerabilities associated with their network devices are applied. 
    - Migrate to new technology to replace the Smart Install legacy protocol, specifically Cisco Network Plug and Play for securely configuring new switches.
    - Network administrators should consult Cisco's Guide to Harden Cisco IOS Devices for additional instructions for improving device security.

## Associated Techniques

- [T1190](https://attack.mitre.org/techniques/T1190) - Exploit Public-Facing Application
- [T1602.002](https://attack.mitre.org/techniques/T1602/002) - Data from Configuration Repository: Network Device Configuration Dump

## Related Countermeasures

No Related Countermeasures content identified.

## References

-   Smart Install Descriptions - Restrictions for Smart Install | <https://www.cisco.com/c/en/us/td/docs/switches/lan/smart_install/configuration/guide/smart_install/concepts.html#23355>

