# Reset NT Hashes for Smart Card-enabled Accounts

* **ID:** CM0029
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; NT hashes and smart card accounts

## Intended Outcome

Resetting NT hashes for smart card-enabled accounts restricts adversary
defense evasion and lateral movement using pass-the-hash attacks.

## Introduction

No Introduction content identified.

## Preparation

- Assess the potential disruption refreshing smartcards/personal identity verification cards will have on the organization prior.
- Verify that the adversary is inhibited from performing subsequent pass-the-hash attacks in the environment so that the adversary does not respond to this mitigation by simply dumping the hashes after a refresh is performed.
- Ensure that the action will not generate a large number of re-authentication requests that risk overloading the Domain Controllers.

## Risks

- This countermeasure may disrupt operations.
- Users of smartcard/PIV-enabled accounts will need to re-associate their smartcard with their account. This re-association can be disruptive if large numbers of users are affected. 
- Refreshing NT hashes will, by itself, not be sufficient to evict a adversary from a compromised network. An adversary must be fully evicted from a network for this countermeasure to be effective. Otherwise, a returning adversary with the requisite privileges may be able to engage in repeated credential theft of LM hashes.

## Guidance

### Automated Method 

- Consult the NSA’s Pass-the-Hash Guidance.
- Use either the NSA’s Invoke-SmartcardHashRefresh PowerShell cmdlet from the “PtHTools” module to change the underlying NT hash or the DoD PKI-PKE provided script under PKI and PKE Tools.

### Manual Method

- To manually update the NT hash for a specific account, disable and re-enable the “Smart Card required for interactive logon” (SCRIL) option.  
- The manual method is not scalable for large groups of accounts.

## Associated Techniques

- [T1003](https://attack.mitre.org/techniques/T1003) - OS Credential Dumping
- [T1550.002](https://attack.mitre.org/techniques/T1550/002) - Use Alternate Authentication Material: Pass the Hash
- [T1555](https://attack.mitre.org/techniques/T1555) - Credentials from Password Stores

## Related Countermeasures

- CM0018 - Enable Active Directory (AD) Protected Users Security Group
- CM0033 - Reset User Account Passwords
- CM0098 - Enable Extended Protection for Authentication (EPA)
- CM0104 - Enable Interactive Logon and Require Smart Card Policy for Privileged User Accounts

## References

- NTLM Overview | <https://learn.microsoft.com/en-us/windows-server/security/kerberos/ntlm-overview>
- Password-less strategy: Configure user accounts to disallow password authentication | <https://learn.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/passwordless-strategy#configure-user-accounts-to-disallow-password-authentication>
- What's new in Credential Protection | <https://learn.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection>
