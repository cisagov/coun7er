# Configure Tactical Privileged Access Workstation

* **ID:** CM0059
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Privileged access administration; hardened admin workstations

## Intended Outcome

Configuring a tactical privileged access workstation (PAW) enables privileged administrators to respond to an incident while minimizing the exposure of privileged administrator accounts.

## Introduction

A privileged access workstation (PAWs) is a dedicated computer environment that allows accounts with elevated permissions, such as Tier-0 domain administrators, to access and configure highly-sensitive accounts, resources, and functions. PAWs are deployed to enforce the separation of security tiers.  Properly configured PAWs ensure the device, accounts, and tools exist within the same security tier, thus minimizing the potential attack surface.

If properly implemented, the tactical PAWs will enable privileged administrators to respond with the required degree of privilege while reducing the risk of exposure from other security tiers. Note that the tactical deployment of PAWs will not protect an environment from an adversary that has already achieved privileged access.

## Preparation

- Ideally, PAWs will be configured prior to an incident as configuring dedicated PAWs can be time consuming. If this in not the case, PAWs will need to be rapidly configured with dedicated hardware and software to enable incident response and reduce exposure.


## Risks

- Leveraging a fresh PAW to facilitate incident response may result in unforeseen difficulties, including software components breaking or security controls interfering with what a responder needs to accomplish to remediate the compromise.

## Guidance

- Create a secure administrative Active Directory organizational unit (OU) structure to host the privileged access workstation (PAW).

- Implement Microsoft Windows Privileged Access Workstation (PAW) Security Technical Implementation Guides (STIG) to quickly minimize the attack surface area of PAWs. The Windows PAW STIG provides configuration and installation requirements for dedicated Windows workstations used exclusively for remote administrative management of designated high-value IT resources.

- Treat the PAW as if it were an air-gapped machine. This means ensuring all remote access is disabled and blocked to prevent unauthorized access.

- Configure the domain administrator account (or other accounts with similarly expansive privilege sets) to only permit authentication from the newly provisioned PAWs. Continue to abide by the principle of least privilege. 

- Ensure firewall rules permit only communications with the AD server and utilize a jump box / jump server between the PAW and compromised computing environment. All outbound connections to the Internet from the PAW should be blocked. 

- Ensure physical hardware supports the latest Trusted Platform Module and encryption.

- Require multi-factor authentication (MFA) for all accounts operating on PAW(s).

- Avoid the use of wireless network communications and rely on wired network connections wherever possible. 

- Document use of USB storage media or peripherals. Avoid reuse of peripherals that were ever used on the compromised computing environment.  

- Only run the necessary tooling to carry out system administration duties to minimize its attack surface. Adding unnecessary tools will risk introducing potential vulnerabilities to an otherwise secure workstation. 

## Associated Techniques

This countermeasure can affect several ATT&CK techniques. These include the following:

- [T1003](https://attack.mitre.org/techniques/T1003/) - OS Credential Dumping
- [T1056](https://attack.mitre.org/techniques/T1056/) - Input Capture
- [T1098](https://attack.mitre.org/techniques/T1098/) - Account Manipulation 	
- [T1606](https://attack.mitre.org/techniques/T1606/) - Forge Web Credentials
- [T1606.002](https://attack.mitre.org/techniques/T1606/002/) - SAML Tokens
- [T1078.002](https://attack.mitre.org/techniques/T1078/002/) - Domain Accounts
- [T1189](https://attack.mitre.org/techniques/T1189/) - Drive-by Compromise
- [T1566](https://attack.mitre.org/techniques/T1566/) - Phishing

## Related Countermeasures

- CM0065 - Isolate Endpoints from Network
- CM0027 - Rebuild Compromised Host
- CM0084 - Restrict Accounts with Privileged Active Directory (AD) Access from Logging into Endpoints
- CM0094 - Restrict Domain Admin User Rights and Monitor Protected Users on Workstations

## References

- Implementing a Zero Trust strategy after compromise recovery | <https://www.microsoft.com/en-us/security/blog/2022/09/14/implementing-a-zero-trust-strategy-after-compromise-recovery/>
- Privileged access: Strategy | <https://learn.microsoft.com/en-us/security/privileged-access-workstations/privileged-access-strategy>
- Privileged access deployment | <https://learn.microsoft.com/en-us/security/privileged-access-workstations/privileged-access-deployment>
- Microsoft Windows Privileged Access Workstation (PAW) STIG - Ver 2, Rel 3 | <https://public.cyber.mil/u_ms_windows_paw_v2r3_stig/>
- Using Privileged Access Workstations (PAWs) to Protect the Cloud | <https://www.beyondtrust.com/blog/entry/using-privileged-access-workstations-to-protect-the-cloud>
