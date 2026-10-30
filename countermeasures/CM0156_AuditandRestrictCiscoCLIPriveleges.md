# Audit and Restrict Cisco CLI Privileges

* **ID:** CM0156
* **Version:** 0.1
* **Created:** 11 February 2025
* **Modified:** 18 March 2025
* **Countermeasure Type:** Examine, Enable
* **Status:** Active
* **Nice Work Role:** Network Operations (IO-WRL-004)
* **Skills & Knowledge:** Cisco network administration; CLI privilege levels and command authorization

## Intended Outcome

Restricting command line execution across Cisco devices to privileged levels prevents adversary objectives in privilege escalation and discovery by accessing running device configurations.

## Introduction

Cisco devices have 16 privilege levels (0-15) that vary permissioned usage of the command line from only five commands (Level 0), all the way up to complete administrative control of the Cisco device (Level 15). Restricting access to specific commands through privileged EXEC mode can prevent certain users from accessing the running configuration file which contains sensitive information such as settings for interfaces, routing protocol information, along with network security features. If an adversary is able to access the running configuration file(s) of the network device, modifications and querying of the file(s) is likely to occur which can lead to unwarranted export of data and credentials. To provide an example of an attacker's malicious modification actions: an attacker may use the `copy` command to transfer a network configuration file between memory locations or network servers that are unauthorized to receive another device's configuration file(s) such as `running-config` or `startup-config.` Privileged EXEC mode exists to prevent this potential for abuse, by requiring a user to supply a password for entry and access to higher-level configuration commands.

## Preparation

- Determine the potential attack surface across the network by examining the quantity and complexity of passwords across network devices.
- Following NSA recommendation, break down all existent passwords into their respective Type classification (0, 4, 5, 6, 7, 8, 9).

## Risks

- Adjusting permissions surrounding privileged modes through the Cisco CLI can introduce access and configuration limitations to Cisco network devices.

## Guidance

### Enforce Privilege Levels to Restrict Access

- Network administrators should customize privilege levels to restrict the execution of specific commands.
- Privilege levels can be created at a command-specific level. 
	- Example:
	  - `Router (config) # privilege_mode level [0-15] command_string`
	  - `Router (config) # enable secret level [0-15] password_string`
	  - `Router (config) # exit`

- Executing these commands will allow the user to specify a privilege level (0-15) to assign to the desired command (`command_string`). An encrypted password can then be applied for the assigned privilege level, such that the user must supply the assigned `password_string` when attempting to execute the `command_string`.
## Associated Techniques

- [T1098](https://attack.mitre.org/techniques/T1098/) - Account Manipulation
- [T1016](https://attack.mitre.org/techniques/T1016) - System Network Configuration Discovery
- [T1059.008](https://attack.mitre.org/techniques/T1059/008/) - Command and Scripting Interpreter: Network Device CLI
- [T1602.002](https://attack.mitre.org/techniques/T1602/002/) - Data from Configuration Repository: Network Device Configuration Dump

## Related Countermeasures

- RM0328 - Implement Cryptographic Password Storage Algorithms
- RM0329 - Audit and Restrict ApplicationImpersonation Role
- RM0332 - Implement Complex Password Policy

## References

- Cisco Password Types: Best Practices | <https://media.defense.gov/2022/Feb/17/2002940795/-1/-1/1/CSI_CISCO_PASSWORD_TYPES_BEST_PRACTICES_20220217.PDF>