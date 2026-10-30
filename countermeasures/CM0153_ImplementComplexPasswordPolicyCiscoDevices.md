# Implement Complex Password Policy for Cisco Devices

* **ID:** CM0153
* **Version:** 0.1
* **Created:** 11 February 2025
* **Modified:** 18 March 2025
* **Countermeasure Type:** Enable
* **Status:** Active
* **Nice Work Role:** Network Operations (IO-WRL-004)
* **Skills & Knowledge:** Cisco network administration; password complexity and privileged access policies

## Intended Outcome

Auditing passwords under robust password complexity requirements prevents adversary privilege escalation and persistence via abuse of privileged EXEC mode across Cisco devices.

## Introduction

It is not uncommon for modern adversaries to obtain hashed passwords and sensitive authentication information from configuration files across Cisco network devices. The presence of passwords across network devices increases the risk of device exploitation. The National Security Agency informs that compromise of network devices is correlated with poor password choice, improper hashing of configuration files, and password reuse. By default, Cisco networking devices contain a plaintext configuration file that is loaded after the Cisco operating system boots. This configuration file contains sensitive data that controls the behavior of the Cisco device, the direction of traffic within a network, as well as user authentication information.

## Preparation

- Determine the potential attack surface across the network by examining the quantity and complexity of passwords across network devices.
- Following NSA recommendation, break down all existent passwords into their respective Type classification (0, 4, 5, 6, 7, 8, 9).
- Document and retire all passwords that fit password type 0, 4, and 7 classification. Upgrade the passwords to Type 6, 8, or 9 classification if software and hardware permits.

## Risks

- Improperly configuring passwords under faulty password policy can result in the usage of various passwords that fall into insecure Type 0, 4, and 7 categories.
- Weak password policy can incite network compromise if an adversary is able to brute force or break the password assigned to Cisco's privilege EXEC mode.

## Guidance

### Configure Password Policy in Privileged EXEC Mode

- In general, strong passwords are those that require considerable effort to be cracked and converted to plaintext. Complex password policy underlines the following features:
	- Passwords contain a combination of lowercase and uppercase letters, symbols, and numbers.
	- Passwords are at least 15 alphanumeric characters.
	- Passwords are NOT:
		- A keyboard walk
		- Equivalent to a user name
		- The default password
		- Redundant- the same as a password used anywhere else
		- Related to the network, organization, location, or other function identifiers
		- Derived from a dictionary or common acronym.
- The NSA specifically recommends that passwords are established under the following constraints:
    - Use Multi-factor authentication when feasible 
    - Apply Password Type 8 for passwords
    - Apply Password Type 6 for VPN keys 
    - Standardize creation of strong, unique passwords
    - Implement Privilege levels for least privilege 
- Password policy should be edited through the Cisco CLI to account for the aforementioned specifications:
	- `Router (config) #aaa new-model`
	- `Router (config) #aaa common-criteria policy policy_name`
	- `Router (config-cc-policy) #char changes <number>`
	- `Router (config-cc-policy) #max-length <number>`
	- `Router (config-cc-policy) #min-length <number>`
	- `Router (config-cc-policy) #numeric-count <number>`
	- `Router (config-cc-policy) #special-case <number>`
	- `Router (config-cc-policy) #exit`
	- `Router (config) #username user common-criteria-policy <policy_name> password <password_name>`
	- `Router (config) #exit`

- Executing the command lines above will edit password policy to require complex passwords for each user within the environment. The complexity of the password is adapted by the specifications made to the aforementioned attributes such as `max-length,` `numeric-count,` and `special-case.`

## Associated Techniques

- [T1098](https://attack.mitre.org/techniques/T1098/) - Account Manipulation
- [T1552](https://attack.mitre.org/techniques/T1552) - Unsecured Credentials 
- [T1078](https://attack.mitre.org/techniques/T1078/) - Valid Accounts

## Related Countermeasures

- RM0328 - Implement Password Storage Algorithms for Cisco Devices
- RM0330 - Audit and Restrict Cisco CLI Privileges
- RM0495 - Restrict SYSVOL Credential Theft

## References

- Cisco Password Types: Best Practices | <https://media.defense.gov/2022/Feb/17/2002940795/-1/-1/1/CSI_CISCO_PASSWORD_TYPES_BEST_PRACTICES_20220217.PDF>