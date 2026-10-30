# Disable Bluetooth

* **ID:** CM0051
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Endpoint administration; Bluetooth and device controls

## Intended Outcome

Disabling Bluetooth will prevent adversary objectives of exfiltration and command and control that are achieved by exploiting the Bluetooth protocol.

## Introduction

Bluetooth is a short-ranged wireless technology standard that is used to exchange files between nearby devices or for connecting wireless devices such as headphones. Adversaries have been able to leverage Bluetooth to send data to and from compromised devices in order to exfiltrate sensitive data or send command and control traffic over the connection.

## Preparation

-   Identify Applications and devices that require Bluetooth for operational purposes.
-   Notify Users and Document changes in disabling Bluetooth across devices.

## Risks

Disabling Bluetooth could break some applications or devices that require Bluetooth to function

## Guidance

### Disable Bluetooth via Windows

-   Bluetooth can be turned off via the Window's settings

-   Another option can be via the action center in the bottom right of the taskbar. Open it and click on the Bluetooth icon.

-   For a more permanent solution, one can go into device manager and disable the Bluetooth device outright.

-   Another option is the Bluetooth User Support Service which can be stopped or disabled in the services menu.

### Disabling Bluetooth via Configuration Manager for MDM devices

-   Create a new configuration item in the Configuration Manager console.

-   In the configuration item creation wizard, there are multiple device settings to choose from that let you restrict or outright disable Bluetooth.

## Associated Techniques

-   [T1011.001](https://attack.mitre.org/techniques/T1011/001/) - Exfiltration Over Other Network Medium: Exfiltration Over Bluetooth

## Related Countermeasures

-   CM0052 - Disable NetBIOS Service
-   CM0053 - Disable Link-Local Multicast Name Resolution (LLMNR) Service

## References

-   Turn Bluetooth off in Windows | <https://support.microsoft.com/en-us/windows/turn-bluetooth-on-or-off-in-windows-9e92fddd-4e12-e32b-9132-5e36bdb2f75a>
-   Additional Ways to Turn Bluetooth off in Windows | <https://www.makeuseof.com/turn-off-bluetooth-on-windows-10/>
-   Windows Bluetooth Features | <https://learn.microsoft.com/en-us/windows-hardware/design/component-guidelines/bluetooth>
-   Create configuration items for Windows devices | <https://learn.microsoft.com/en-us/mem/configmgr/mdm/deploy-use/create-configuration-items-for-windows-8.1-and-windows-10-devices-managed-without-the-client>