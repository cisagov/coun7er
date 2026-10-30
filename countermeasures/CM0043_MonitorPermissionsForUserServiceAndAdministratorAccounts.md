# Monitor Permissions for User, Service, and Administrator Accounts

* **ID:** CM0043
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Identity administration; account permissions and access reviews

## Intended Outcome

Monitoring permissions for user, service, and administrator accounts mitigates adversary privilege escalation using valid accounts.

## Introduction

The approach, order, and speed to which organizations audit privileges will vary depending on context-specific factors such as the organization's environment, constraints, profile, and obligations or the attacker's activity within the environment. Because every incident is different, attackers are varied, and organizations diverse, an organization must consider this countermeasure in light of its specific circumstances. 

## Preparation

- Gain a thorough understanding of the user account environment.
- Ensure access to an administrator account as required by User Access Control mechanisms.
- Verify which user groups are available and confirm the specific permissions for each group; differentiate between standard user and administrator accounts.

## Risks

- Incorrectly identifying users requiring elevated privileges as standard users may break legitimate functionality.

## Guidance

While implementing this countermeasure, ensure close coordination between onsite/local and remote system administrators. 

### Verify that Privilege Elevation is Disabled for Standard User Accounts

Steps to verify proper disabling of privilege elevation for standard users varies depending on the specific implementation of an organization’s architecture (use of Active Directory, custom user groups deployed, etc.). The following methods can be used to confirm details regarding user accounts, including whether privilege elevation is disabled for standard users.

- Use the Windows Settings application to identify the type of account of different users that are available on the local workstation.
- Use the Computer Management tool to identify Local Users and Groups and review properties associated with users of interest. Check that the groups a user is a member of do not contain administrator level privileges or other privileges that should not be assigned to standard users.
- The Local Security Policy tool can help control certain behaviors of the User Account Control settings, including whether standard users are prompted for elevated credentials when attempting to complete an action that requires elevated privileges or whether they are denied automatically (standard users should be denied automatically). 

### Audit Account Privileges in Active Directory

- Inventory roles and groups that possess privileged security levels in the environment.
	- For on-prem AD, use on-prem Active Directory or PowerShell to obtain a list privileged users in a domain. 
	- For Azure AD, use Azure AD Privileged Identity Management (PIM) or the PowerShell API.

#### Adopt a privileged access strategy

- Azure AD customers should follow the rapid modernization plan (RAMP) to quickly adopt Microsoft’s privileged access strategy if they aren’t already.

### Audit Relationships with Cloud Service Providers (CSPs)

- In cloud environments, ensure permissions auditing includes shared trusts or identity relationships with third-party CSPs in which the identity is a resident on the CSP’s tenant but is also capable of performing actions in the organization’s M365 environment.
    - Prioritize accounts associated with the threat actor
    - Prioritize accounts that were associated with the compromised administrators, services, and users known to be leveraged by the adversary. 
    - Ensure SIEMs are configured to receive Windows event logs and exploit these logs to analyze events associated with the use or creation of privileged actions. 

### Ensure Highly Privileged Accounts are Delegated Just Enough Administration (JEA)

Considerations should exceed to what is immediately associated with the incident: 

- Focus recovery and response efforts on accounts belonging to highly privileged groups or recently obtained, undocumented privileges. These accounts usually have full administrative rights, such as Domain admins, Enterprise admins, and Schema admins, and are members of the forest root domain. 
- Explore all other accounts with limited, but still significant, rights (Account Operators, Server Operators, DNSAdmins) are also an important priority. 
- For built-in groups and custom groups, PowerShell can be used to enumerate their membership. When a highly privileged account is identified, consider granting the minimal rights and accounts needed to function, following the principles of JEA.

### Review Access Control Lists to Identify Potential Backdoor Access

When auditing privileges, review access control entries to ensure they are delegated according to the Principle of Least Privilege (PoLP) to limit the risk of backdoor access to additional powers. See the following guidance for recommendations when auditing AD access control listings.

- Consult as a team to determine if an account’s permissions should be revoked.
- Determine whether each identified account of concern should maintain their current privileges. 
- When conducting the assessment, abide by the principle of least privilege. 
- Revoke all accounts that are unnecessary or possess privileges not expressly required by an account’s function.
- Accounts identified as having passwords past a certain age should be rotated.

### Respond to Excessive Privileges

- If accounts with excessive privileges are identified, revoke privileges assigned to the user or group.  If the account is unmanaged, decommission the account.  
- Document any changes to privileges.  

### Investigate the utility of 3rd-Party solutions

Organizations conducting account privilege auditing should consider the applicability of third-party and opensource tools to streamline the audit process, particularly for highly complex environments. While every tool may not be appropriate for every organization, teams should explore what tools are available and whether they can be used for their environments. Some possibilities include:

- AD ACL Scanner – PowerShell tool used to create reports of access control lists and system access control lists in Active Directory. Useful for rapidly comparing Access Control Lists using update sequence number (USN) from replication metadata. 
- BloodHound – Uses graph theory to identify relationships within an within an Active Directory or Azure environment, allowing defenders to understand privilege relationships in their environment to revoke where appropriate. 
- Lithnet Access Manger – opensource software designed to allow the delegation of sensitive administrative access to computers for on-prem environments.

In addition, many commercial tools can also be used to perform privilege access management auditing and can be used to generate reports. 

After completion of a privilege audit, organizations should plan to regularly review and update privileges on an ongoing basis. More comprehensive assessments of account privileges should be scheduled in a timeline informed by the time and budgetary constraints of the organization.

## Associated Techniques

- [T1548.002](https://attack.mitre.org/techniques/T1548/002) - Abuse Elevation Control Mechanism: Bypass User Account Control
- [T1078](https://attack.mitre.org/techniques/T1078/) - Valid Accounts

## Related Countermeasures

- CM0003 - Configure File and Directory Permissions
- CM0048 - Disable Hyper-V
- CM0018 - Enable Active Directory (AD) Protected Users Security Group
- CM0060 - Restrict and Monitor Domain Accounts with Local Administrator Access to Workstations
- CM0028 - Reset Service Account Passwords
- CM0085 - Audit Group Policy Objects (GPOs) in Active Directory (AD)
- CM0033 - Reset User Account Passwords
- CM0044 - Monitor Account Creation and Permission Changes
- CM0112 - Remove Extraneous and Stale Accounts
- CM0114 - Enable Microsoft Conditional Access Policy Templates
- CM0118 - Revoke Existing Administrator Permissions and Deploy New Administrator Accounts
- CM0124 - Disable On-Premise Active Directory (AD) Accounts with Privileged Roles in Entra ID
- CM0136 - Configure and Invalidate Relative ID (RID) Pools
- CM0139 - Rebuild Active Directory (AD) Connect Server
- CM0145 - Reset Interdomain Trust Account (ITA) Credentials

## References

- User Account Control settings and configuration | <https://learn.microsoft.com/en-us/windows/security/identity-protection/user-account-control/user-account-control-security-policy-settings>
- least privilege | <https://csrc.nist.gov/glossary/term/least_privilege>
- Privileged access: Strategy | <https://learn.microsoft.com/en-us/security/privileged-access-workstations/privileged-access-strategy>
- Just Enough Administration | <https://learn.microsoft.com/en-us/powershell/scripting/security/remoting/jea/overview?view=powershell-7.4>
- "Admin Free" Active Directory and Windows, Part 1- Understanding Privileged Groups in AD | <https://learn.microsoft.com/en-us/archive/blogs/lrobins/admin-free-active-directory-and-windows-part-1-understanding-privileged-groups-in-ad>
