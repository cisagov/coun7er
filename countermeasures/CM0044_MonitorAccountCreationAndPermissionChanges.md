# Monitor Account Creation and Permission Changes

* **ID:** CM0044
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active

## Intended Outcome

Monitoring account creation and permission changes detects adversary
persistence and privilege escalation using valid accounts.

## Introduction

Threat actors will often create a local or domain account to maintain
persistence in an environment. Additionally, account takeover is often
followed by privilege escalation, so auditing known accounts for
elevated privileges is necessary.

## Preparation

This mitigation is applicable to multiple platforms at both the local
and domain level. Administrators should not use this mitigation as
prescriptive guidance, but rather descriptive guidance. The examples
herein refer to the Windows operating system, but the context is
applicable to other environments with minor changes.

## Risks

- New or undocumented accounts may be valid; deleting them may cause operational disruption.

## Guidance

### Permissions

Once an adversary has access to an account, they will likely try to
elevate their privileges. During an audit, check user SIDs and map to a
baseline configuration file for users. Users whose SIDS equal System or
Admin level without being a known Administrator or Service account
should be flagged for further review. A deeper inspection would be to
check to see if users are running binaries, programs or processes with
elevated privileges.

### Creations

Adversaries may also create their own rogue account in order to maintain
persistent access to an environment. Compare "known users" with users
present in an environment and flag unknown users.

### Incident Response

During an ongoing incident, check for recently created accounts on local
systems and in a domain. Compare accounts to a secure baseline,
on-boarding documentation, etc. Suspicious accounts should be disabled
and further investigated for activity performed on systems within the
network. Moreover, current accounts that are performing actions outside
of the scope of the account type should be disabled. The relative
identifier which is the last number of the SID can indicate at what
level an account is running. For example, 512 is the identifier for a
user belonging to the Domain Admins group. Accounts that should not
belong in this group should be disabled and further investigated.
Lastly, Microsoft's EventViewer is an excellent tool for auditing
accounts.

## Associated Techniques

- [T1078](https://attack.mitre.org/techniques/T1078) - Valid Accounts
- [T1098](https://attack.mitre.org/techniques/T1098) - Account Manipulation
- [T1136](https://attack.mitre.org/techniques/T1136) - Create Account

## Related Countermeasures

- CM0028 - Reset Service Account Passwords
- CM0068 - Audit and Restrict Exchange ApplicationImpersonation Role
- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts
- CM0112 - Remove Extraneous and Stale Accounts
- CM0114 - Enable Microsoft Conditional Access Policy Templates
- CM0118 - Revoke Existing Administrator Permissions and Deploy New Administrator Accounts
- CM0136 - Configure and Invalidate Relative ID (RID) Pools
- CM0140 - Configure Windows Audit Policy

## References

- Security Identifiers | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-identifiers>
- Security principals | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-principals>
- Active Directory security groups | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-groups>
- Active Directory accounts | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-default-user-accounts>
- Audit User Account Management | <https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/audit-user-account-management>
