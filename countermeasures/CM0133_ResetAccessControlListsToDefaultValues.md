# Reset Access Control Lists to Default Values

* **ID:** CM0133
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Systems administration; access control lists and permission restoration

## Intended Outcome

Resetting Access Control Lists (ACLs) to default values blocks or restricts an adversary's attempts at persistence, privilege escalation, and defense evasion using created accounts or modified permissions.

## Introduction

ACLs dictate what users or roles can access what resources, and adversaries may modify them to gain further unauthorized access. For example, an adversary can modify an ACL to add an adversary-owned account to a privileged group, facilitating persistent privileged access to the environment that a password reset may not eliminate. 

## Preparation

- Audit access control policies for the organization.
- Identify default access control settings for roles, users, and services in the environment or locate Microsoft's default access permission list.
- Ensure event logging is enabled. 

## Risks

- Organizations that had not updated their ACL policies may have to revert to a prior, documented default and some users may lose access to resources needed for their work. Reset ACLs during off hours and update resulting roles and permissions as necessary. 

## Guidance

Microsoft provides a list of default access permissions for different user groups and roles. 

- Audit role permissions to ensure expected values.
- Audit user and group assignments to ensure expected values.
- If necessary, delete existing groups, create new groups with appropriate roles and permissions, and re-assign users.
	- In the Microsoft Entra admin center, navigate to `Identity > Groups > All groups` and select `New group`.
	- Select or fill in appropriate settings such as group name and description. 
	- Switch `Microsoft Entra roles can be assigned to the group` setting to `yes` and add appropriate roles. 
	- Add users to the group. 
- Monitor for access control permission changes: configure central access policy monitoring for files and directories.
	- In the Group Policy management console, navigate to `Computer Configuration > Security Settings > Advanced Audit Policy > Policy Change > Audit Authorization Policy Change`.
	- In the `Configure the following audit options`, select all desired options ("Success" or "Failure" or both). 
	- Watch for event logs with ID 4913. 
- To monitor for changes to specific files or directories:
	- Sign in as an administrator on machine containing the file or directory. 
	- Select file/directory's `Properties > Security > Advanced > Auditing` tab, then select `Continue`. 
	- Add the appropriate service principal, then select permissions to audit in the `Auditing Entry for` dialog box. 
	- Update policy with the `gpupdate /force` command. 

## Associated Techniques

- [T1078](https://attack.mitre.org/techniques/T1078) - Valid Accounts
- [T1136](https://attack.mitre.org/techniques/T1136) - Create Account
- [T1190](https://attack.mitre.org/techniques/T1190) - Exploit Public-Facing Application
- [T1222](https://attack.mitre.org/techniques/T1222) - File and Directory Permissions Modification

## Related Countermeasures

- CM0003 - Verify/Configure File and Directory Permissions
- CM0075 - Implement Access Restrictions on Cloud Storage Objects
- CM0085 - Audit Group Policy Objects (GPOs) in Active Directory (AD) 

## References

- How Access Control Works in Active Directory Domain Services | <https://learn.microsoft.com/en-us/windows/win32/ad/how-access-control-works-in-active-directory-domain-services>
- What are the default user permissions in Microsoft Entra ID? | <https://learn.microsoft.com/en-us/entra/fundamentals/users-default-permissions>
- Manage Microsoft Entra groups and group membership | <https://learn.microsoft.com/en-us/entra/fundamentals/how-to-manage-groups>