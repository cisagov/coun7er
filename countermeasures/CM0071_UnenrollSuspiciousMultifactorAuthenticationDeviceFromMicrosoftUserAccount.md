# Unenroll Suspicious Multifactor Authentication (MFA) Device from Microsoft User Account

* **ID:** CM0071
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Eliminate
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Microsoft Entra ID administration; MFA devices and authentication methods

## Intended Outcome

Unenrolling a suspicious Multifactor Authentication (MFA) device from a Microsoft user account terminates adversary persistence to environments granted by the device.

## Introduction

MFA forces users to prove their digital identity to a system using two or more factors such as an approved smartphone and/or linked smartcard.

To circumvent this defense, adversaries may take control over a target's MFA device. Examples of this include but are not limited to:

- Registering an adversary's own device as a form of authentication to maintain persistence in an environment.
- Performing SIM swapping to register the attacker's device with the target's phone number in order to obtain the mobile authentication codes delivered via SMS. 
- Deploying malware to a target's mobile device to intercept SMS call logs, key logs, notifications, and application authenticator codes.

When a sophisticated adversary is able to bypass MFA protections to hijack a valid account, administrators and individuals with mobile-device management authorities should consider revoking that device and enrolling the user in a new method of MFA. 

## Preparation

- System administrators should rotate passwords before removing a suspicious MFA device and enrolling a new MFA device. 
- After an incident, responders should investigate user accounts with new or suspicious device associations. Devices associated with unusual times, sources, or suspicious logins should be prioritized. 

## Risks

- Unenrolling a suspicious MFA device may disrupt user access to their account until a new device is registered.
- Unenrolling an MFA device might risk temporarily disabling MFA and enabling password-only access until a new multifactor device is registered. 

## Guidance

### Unenroll MFA Device

#### Unenroll MFA Device via Microsoft 365 and Entra ID

Within the Entra admin center portal, administrators can navigate to Users > [Name] and Under Manage select "Authentication methods." On this page, an individual user's usable authentication methods may be changed, password reset, MFA device re-registered, and MFA session revoked. 

#### Unenroll MFA Device with Graph API

Rather than using the web interface, administrators may also leverage Microsoft Graph API to interface with Microsoft Entra ID and modify user authentication methods. Administrators should identify which authentication method the adversary bypassed, consult Microsoft documentation for the appropriate authentication method, and identify the relevant API to invoke to unenroll the compromised device and/or replace the method with an alternate, more secure means of authentication. 

### Review and Update Conditional Access Policies

System administrators should review existing Conditional Access policies to identify potential gaps and misconfigurations in light of MFA bypass and pay particular attention to privileged and Tier 0 accounts. 

### Continue Enforcing MFA

After removing a suspicious or compromised MFA device, users should continue utilizing MFA for their logins.

If the removal of an adversary's MFA device temporarily permits the user to login without MFA, enable a new device for the user as soon as possible. 

Organizations should continue enforcing MFA following an incident. Any subsequent changes to an organization's existing MFA policy will depend on how the adversary achieved authentication. For example, if an adversary intercepted a two factor authentication (2FA) code sent via SMS as a result of SIM swapping, organizations should consider rolling out application or hardware-based MFA solutions. Alternately, if the adversary managed to enroll their MFA device to a compromised account, teams should review their alerts or introduce new rules to alert on future, suspicious device registrations and duplicate MFA device associations. 

## Associated Techniques

- [T1098.005](https://attack.mitre.org/techniques/T1098/005/) -  Account Manipulation: Device Registration 
- [T1556.006](https://attack.mitre.org/techniques/T1556/006/) - Modify Authentication Process: Multi-Factor Authentication 
- [T1621](https://attack.mitre.org/techniques/T1621/) -  Multi-Factor Authentication Request Generation 
- [T1111](https://attack.mitre.org/techniques/T1111/) - Multi-Factor Authentication Interception 

## Related Countermeasures

- CM0018 - Enable Active Directory (AD) Protected Users Security Group
- CM0033 - Reset User Account Passwords
- CM0112 - Remove Extraneous and Stale Accounts
- CM0118 - Revoke Existing Administrator Permissions and Deploy New Administrator Accounts
- CM0121 - Enable Managed Domain Authentication
- CM0124 - Disable On-Premise Active Directory (AD) Accounts with Privileged Roles in Entra ID
- CM0126 - Reset Credentials for Local Administrator Accounts Twice
- CM0128 - Reset Credentials for Global and Domain Administrator Accounts Twice

## References

- Multifactor authentication for Microsoft 365 | <https://learn.microsoft.com/en-us/microsoft-365/admin/security-and-compliance/multi-factor-authentication-microsoft-365?view=o365-worldwide>
- Manage device identities using the Microsoft Entra admin center | <https://learn.microsoft.com/en-us/entra/identity/devices/manage-device-identities>
- Microsoft Entra authentication methods API overview | <https://learn.microsoft.com/en-us/graph/api/resources/authenticationmethods-overview?view=graph-rest-1.0>

