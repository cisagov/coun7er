# Reset and Monitor adminCount Attribute

* **ID:** CM0125
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; adminCount attributes and protected account permissions, Powershell

## Intended Outcome

Resetting and monitoring the adminCount attribute blocks or prevents an adversary's attempts to achieve persistence or privilege escalation by abusing properties of adminCount. 

## Introduction

The adminCount attribute indicates that an object's access control lists (ACLs) have been changed by the system to a more secure setting since it is (or was) a member of a privileged or administrative group. On privileged user accounts, this attribute is set to 1. That disables inheritance on that object or account, and sets the security to be governed by the AdminSDHolder object. AdminSDHolder runs the SDProp process every hour to reset permissions to the configured set. If an adversary edits those permissions, they may get reverted back to a compromised state even after attempted remediation.

An adversary can modify the value of the adminCount attribute on any account to prevent password resets using default means, making it more difficult to remove their access to that account. They may also modify the attribute for legitimate administrator accounts to prevent system administrators from performing their tasks successfully.    

## Preparation

- Review access control policies and determine which accounts or objects should have adminCount set to 1.

## Risks

- Changing the adminCount attribute to 1 may affect inheritance of accounts. Be aware of what accounts may need to inherit permissions from organizational methods, as implementation of the inheritance may be difficult.

## Guidance

Find all user accounts with adminCount set to 1 with the following command: `Get-aduser -Filter *  -prop admincount, Canonicalname | where admincount -eq 1 | select Name, SamAccountName, AdminCount, Canonicalname`.

- For all accounts that should not have this attribute set, reset it to 0.
- For any accounts that should have the attribute set but are not in the list, reset it to 1.

### Monitor for attempts to reset the adminCount attribute from PowerShell

- Monitor all process command lines containing `admincount`.
- Monitor all process command lines containing `set-aduser`.

## Associated Techniques

- [T1078](https://attack.mitre.org/techniques/T1078) - Valid Accounts
- [T1212](https://attack.mitre.org/techniques/T1212) - Exploitation for Credential Access 

## Related Countermeasures

- CM0018 - Enable Active Directory (AD) Protected Users Security Group
- CM0063 - Investigate Suspicious Login Attempts
- CM0031 - Monitor PowerShell Program
- CM0085 - Audit Group Policy Objects (GPOs) in Active Directory (AD)
- CM0033 - Reset User Account Passwords
- CM0139 - Rebuild Active Directory (AD) Connect Server

## References

- Admin-Count attribute | <https://learn.microsoft.com/en-us/windows/win32/adschema/a-admincount>
- Learn to adjust the AdminCount attribute in protected accounts | <https://www.techtarget.com/searchwindowsserver/tutorial/Learn-to-adjust-the-AdminCount-attribute-in-protected-accounts>
