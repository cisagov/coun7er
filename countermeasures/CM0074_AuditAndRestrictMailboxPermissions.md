# Audit and Restrict Mailbox Permissions

* **ID:** CM0074
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine, Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Microsoft Exchange administration; mailbox permissions and access reviews

## Intended Outcome 

Auditing and restricting mailbox permissions detects and blocks adversary exfiltration using mailboxes.

## Introduction

Adversaries may attempt to modify mailbox account permissions in order to access the contents of a mailbox account or use the compromised mailbox as a staging ground for exfiltration. When responding to an incident involving business email compromise (BEC) or likely compromise of a user’s mailbox, administrators should audit mailbox access logs and confirm additional email permissions were not delegated by the adversary. 

## Preparation

- Administrators should confirm that Exchange mailbox auditing is enabled. This can be done by connecting to the Exchange Server and confirming that auditing is enabled before accessing audit reports from the Microsoft 365 portal. (Get-OrganizationConfig | Format-List AuditDisabled). Note that while mailbox auditing may be enabled, only subscribers with Premium licenses (e.g. E5/A5/G5 licenses) will be able to return mailbox audit log events in audit log searches in the Microsoft Purview compliance portal or via the Office 365 Management Activity API by default. 

## Risks

No Risks content identified.

## Guidance

While delegation is likely possible on any mailbox application, adversaries are most likely to abuse Microsoft Exchange, a component of Microsoft 365 which may be run locally or, more often, as SaaS. The following instructions provide guidance for Microsoft Exchange. 

### Review Audit Logs in Microsoft Exchange

Administrators can investigate mailbox permission modifications in Microsoft 365 by checking the mailbox audit logs. This may be accomplished using different methods. Audit log searches can be conducted using Microsoft Purview or the Office 365 Management Activity API, entering commands in Exchange Online PowerShell, and/or generating reports via Exchange Admin Center (EAC). 

Microsoft Purview Audit (Premium) provides administrator's with access to MailItemsAccessed audit records. These records can guide their hunt for suspicious permission modifications of suspected compromised accounts. 

Alternately, administrators might consider using the EAC to run a non-owner mailbox access report for recent mailbox access. 

Organizations without access to Microsoft Purview Audit (Premium) should still generate an audit log report and search for suspicious use of Microsoft PowerShell cmdlets. The following cmdlets may be suspicious and warrant further investigation, given the right context:

- Add-MailboxPermission: May be misused to add permissions to a mailbox.  
- Set-CASMailbox: May be misused to configure client access settings on a mailbox – such as allowing them to sync mailboxes and copy mail to the adversary’s device(s).  
- Add-MailboxFolderPermission: May be misused to grant permissions to specific mailbox folders (e.g., Inbox, Sent Items). 
- Set-MailboxFolderPermission: May be misused to modify folder-level permissions for users in mailboxes for existing permission entries. 
- Remove-MailboxFolderPermission: May be misused by an adversary to undo mailbox folder permission changes and cover up their tracks. 

Auditing Exchange mailboxes may be accomplished with PowerShell using the following cmdlets:

- Search-UnifiedAuditLog to search the Exchange Online unified audit log and other cloud-based services such as SharePoint Online, Microsoft Teams, and Microsoft Entra ID. 
- Search-MailboxAuditLog to search mailbox audit log entries matching specified search terms. 
- Get-MailboxPermission to retrieve permissions on a mailbox. 
- Get-MailboxFolderPermission (or, for Exchange Online PowerShell, Get-EXOMailboxFolderPermission) to view folder-level permissions. 

Administrators may also want to consult existing PowerShell scripts used to perform mailbox auditing of Microsoft 365 accounts.

### Review Graph API Application Permissions for Mailbox Access

Adversaries that compromise an Azure AD Tenant will be able to create new multi-tenant applications equipped with specific API permissions. Users who unintentionally consent to an applications permissions request may compromise their mailbox, among other possibilities.  For example, an adversary might assign Mail.Read to an application to allow it to read mail in an inbox without a signed-in user. Other potential permissions include:

- Mail.Read
- Mail.ReadBasic
- Mail.ReadBasic.All
- Mail.ReadWrite
- Mail.Send
- MailboxSettings.Read
- MailboxSettings.ReadWrite
- Calendars.Read
- Calendars.ReadWrite
- Contacts.Read
- Contacts.ReadWrite

Teams may need to investigate their Azure logs if they suspect access to a mailbox via suspicious application consent. Microsoft provides hunting queries / analytics to locate this type of behavior, including for the SolarWinds Compromise / Solorigate and the O365 Attack Toolkit.  Alternately, CISA’s Sparrow tool may be used to check the Graph API application permissions of all service principals and applications in M365/Azure AD. 
Administrators can use the New-ApplicationAccessPolicy PowerShell cmdlet to restrict application access to specific mailboxes. 

### Establish an Alert Policy for Mailbox Permission Changes

Administrators should consider establishing an alert policy to receive notifications when administrators assign mailbox permissions. Alerts may be created and viewed either via Microsoft’s Compliance Center or the Microsoft Defender Portal. 

## Associated Techniques

- [T1098.002](https://attack.mitre.org/techniques/T1098/002/) - Account Manipulation: Additional Email Delegate Permissions
- [T1098.005](https://attack.mitre.org/techniques/T1098/005/) - Account Manipulation: Device Registration
- [T1070.008](https://attack.mitre.org/techniques/T1070/008/) - Indicator Removal: Clear Mailbox Data
- [T1114.002](https://attack.mitre.org/techniques/T1114/002/) - Email Collection: Remote Email Collection

## Related Countermeasures

- CM0061 - Rotate Mailbox Passwords
- CM0068 - Audit and Restrict Exchange ApplicationImpersonation Role
- CM0082 - Identify and Remove Suspicious Email Forwarding Rules

## References

- Manage mailbox auditing | <https://learn.microsoft.com/en-us/purview/audit-mailboxes?view=o365-worldwide#more-information>
- Use Microsoft Purview Audit (Premium) to investigate compromised accounts | <https://learn.microsoft.com/en-us/purview/audit-log-investigate-accounts>
- Run a non-owner mailbox access report | <https://learn.microsoft.com/en-us/exchange/policy-and-compliance/non-owner-mailbox-access-reports?view=exchserver-2019>
- Add-MailboxPermission | <https://learn.microsoft.com/en-us/powershell/module/exchange/add-mailboxpermission?view=exchange-ps>
- Set-CASMailbox | <https://learn.microsoft.com/en-us/powershell/module/exchange/set-casmailbox?view=exchange-ps>
- Add-MailboxFolderPermission | <https://learn.microsoft.com/en-us/powershell/module/exchange/add-mailboxfolderpermission?view=exchange-ps>
- Set-MailboxFolderPermission | <https://learn.microsoft.com/en-us/powershell/module/exchange/set-mailboxfolderpermission?view=exchange-ps>
- Remove-MailboxFolderPermission | <https://learn.microsoft.com/en-us/powershell/module/exchange/remove-mailboxfolderpermission?view=exchange-ps>
- Remove-MailboxExportRequest | <https://learn.microsoft.com/en-us/powershell/module/exchange/remove-mailboxexportrequest?view=exchange-ps>
- Search-UnifiedAuditLog | <https://learn.microsoft.com/en-us/powershell/module/exchange/search-unifiedauditlog?view=exchange-ps>
- Search-MailboxAuditLog | <https://learn.microsoft.com/en-us/powershell/module/exchange/search-mailboxauditlog?view=exchange-ps>
- Get-MailboxPermission | <https://learn.microsoft.com/en-us/powershell/module/exchange/get-mailboxpermission?view=exchange-ps>
- Get-MailboxFolderPermission | <https://learn.microsoft.com/en-us/powershell/module/exchange/get-mailboxfolderpermission?view=exchange-ps>
- admindroid-community Enable mailbox auditing in Exchange Online | <https://github.com/admindroid-community/powershell-scripts/blob/master/Enable%20Mailbox%20Auditing%20for%20Office%20365%20Users/EnableMBAuditLogging.ps1>
- Real-Time Alerting with Microsoft 365 Alert Policies | <https://o365reports.com/2022/03/10/real-time-alerting-with-microsoft-365-alert-policies/>
- Limiting application permissions to specific Exchange Online mailboxes | <https://learn.microsoft.com/en-us/graph/auth-limit-mailbox-access>
- Detecting Post-Compromise Threat Activity in Microsoft Cloud Environments | <https://www.cisa.gov/news-events/cybersecurity-advisories/aa21-008a>
