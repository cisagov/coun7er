# Reset Interdomain Trust Account (ITA) Credentials

* **ID:** CM0145
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; interdomain trust accounts and credential rotation

## Intended Outcome

Resetting interdomain trust accounts (ITA) credentials prevents an adversary's attempts at initial access and lateral movement via compromised ITA credentials. 

## Introduction

ITA are objects stored in an authenticating domain containing all the information regarding trust relationships between that domain and a trusted domain. These accounts contain trust secrets of the domains, maintained by the domain controller with the primary domain controller (PDC) Emulator Flexible Single Master Operations (FSMO) role.  

Adversaries can compromise the interdomain trust account in the trusting domain and use its password hash to authenticate in the trusted domain, even if the trust relationship does not allow such access. This process can be abused to gain access to resources on different domains in the environment.

## Preparation

- Identify trust relationships between domains, including trust direction. 
- Identify which relationships have interdomain trust accounts, and which domains those accounts are on.

## Risks

No Risks content identified. 

## Guidance

### Reset ITA credentials

- In the command line interface, use the `netdom experthelp trust` command to get appropriate syntax for the NetDom tool. 
- Use provided syntax to reset the password based on domain configuration. Two examples are provided below. 
	- On parent domain: `netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*`.
	- On child domain: `netdom trust child domain name /domain:parent domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*`. 
- Only run the command once; it automatically resets the password twice. 

### Monitor for ITA hash compromise techniques

- Monitor processes and command-line parameters for evidence of mimikatz.
- Monitor for process execution of Rubeus.
- Monitor for Kerberos ticket-granting ticket (TGT) requests regarding the interdomain trust accounts.

## Associated Techniques

- [T1078](https://attack.mitre.org/techniques/T1078) - Valid Accounts
- [T1199](https://attack.mitre.org/techniques/T1199) - Trusted Relationship
- [T1550.002](https://attack.mitre.org/techniques/T1550/002) - Use Alternate Authentication Material: Pass the Hash   

## Related Countermeasures

- CM0050 - Reset Kerberos Ticket Granting Ticket (KRBTGT) Password Twice
- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts
- CM0121 - Enable Managed Domain Authentication
- CM0123 - Remove Malicious Domain Trust from Tenant
- CM0126 - Reset Credentials for Local Administrator Accounts Twice
- CM0128 - Reset Credentials for Global and Domain Administrator Accounts Twice
- CM0134 - Audit Federated Systems Trust Settings
- CM0146 - Configure Flexible Single Master Operations (FSMO) Roles

## References

- 6.1.6.8 Essential Attributes of Interdomain Trust Accounts | <https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-adts/ac527b5b-0e88-48a1-8c73-497d40388d04>
- Abusing Trust Account$: Accessing Resources on a Trusted Domain from a Trusting Domain | <https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/abusing-trust-accountusd-accessing-resources-on-a-trusted-domain-from-a-trusting-domain>
- SID filter as security boundary between domains? (Part 7) - Trust account attack - from trusting to trusted | <https://blog.improsec.com/tech-blog/sid-filter-as-security-boundary-between-domains-part-7-trust-account-attack-from-trusting-to-trusted>
- Active Directory Forest Recovery - Reset a trust password on one side of the trust | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/forest-recovery-guide/ad-forest-recovery-reset-trust>