# Enable Interactive Logon and Require Smart Card Policy for Privileged User Accounts

* **ID:** CM0104
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; smart card authentication and privileged logon policies

## Intended Outcome

Enabling Interactive logon and requiring smart card policy for privileged user accounts prevents adversaries from logging into privileged accounts via cracked credentials.

## Introduction

Adversaries frequently take advantage of stolen credentials to abuse privileged accounts. Smart cards can add a physical layer of security and two-factor authentication for cards that require personal identification numbers (PINS).

## Preparation

A smart card solution for organization is required including a public key infrastructure, smart cards, and smart card readers. Building and implementing a new smart card solution during remediation for this countermeasure is not recommended.

Determine if the time and resources for the preparation of the this countermeasure are an efficient approach.

Distribution of smart cards and smart card readers to those who need to access the privileged accounts.

## Risks

This countermeasure could result in loss of access to privileged user accounts by Administrators if incorrectly configured.

## Guidance

### Enable Interactive logon: Require smart card via group policy

The setting “Interactive logon: require smart card” (or "Interactive logon: Require Windows Hello for Business or smart card" for windows 10 or newer) refers to a Group policy option that requires users to sign in via a smart card or Windows Hello for Business Method. By default, the policy value that controls this is located in 'Computer Configuration>Windows Settings>Security Settings> Local Policies>Security Options'. Enable this policy for the Domain Admins security group to require privileged users specifically. It is recommended that implementation should be gradually tested and monitored to ensure it is configured correctly and effectively.

### Enabling manually

Individual accounts can also have smart cards required for login via the account's properties tab in Active Directory Users and computer tool. The flag to set can be found account tab in the "Account options" section.


## Associated Techniques

-   [T1078](https://attack.mitre.org/techniques/T1078/) - Valid Accounts
-   [T1552](https://attack.mitre.org/techniques/T1552/) - Unsecured Credentials

## Related Countermeasures

- CM0018 - Enable Active Directory (AD) Protected Users Security Group
- CM0060 - Restrict and Monitor Domain Accounts with Local Administrator Access to Workstations
- CM0029 - Reset NT Hashes for Smart Card-enabled Accounts
- CM0088 - Enable Credential Guard

## References

-   Interactive logon: Require Windows Hello for Business or smart card | <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/security-policy-settings/interactive-logon-require-smart-card>
-   Appendix D: Securing Built-in Administrator Accounts in Active Directory | <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/security-policy-settings/interactive-logon-require-smart-card>
-   KnowledgeBase: Windows Hello for Business satisfies Smartcard is required for interactive logon requirements | <https://dirteam.com/sander/2021/12/03/knowledgebase-windows-hello-for-business-satisfies-smartcard-is-required-for-interactive-logon-requirements/>