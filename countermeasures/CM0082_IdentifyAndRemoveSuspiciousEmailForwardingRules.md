# Identify and Remove Suspicious Email Forwarding Rules 

* **ID:** CM0082 
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine, Eliminate 
* **Status:** Active
* **Nice Work Role:** Incident Response (PD-WRL-003)
* **Skills & Knowledge:** Email incident response; mailbox rules and forwarding analysis

## Intended Outcome  

Identifying and removing suspicious email forwarding rules restricts adversary collection using compromised e-mail accounts. 

## Introduction 

Adversaries that have compromised email accounts may introduce auto-forward rules to collect sensitive information, monitor an account's communications, or maintain persistent access to an account after credentials are reset. When recovering from an incident involving account compromise, investigate suspect email forwarding rules and remove any that may be leveraged by the adversary. 

## Preparation 

- Before removing a forwarding rule, administrators should disable the rule while investigating the sender's past behavior and recent activities to determine whether the account is indeed compromised. This may include reviewing the time, location, and devices used by the account holder.  

- Administrators should block the ability for the affected user(s) to login, reset the user's password(s), revoke the appropriate access session tokens, and take note of the forwarding address before removing forwarding rule(s) introduced by a suspected adversary.  

- When auditing email accounts for suspicious forwarding rules, administrators should verify that mailbox auditing is turned on (auditing is usually on by default). This can be performed using Exchange Online PowerShell.

- If the forwarding address is associated with a suspicious domain, administrators should also consider adding it to their DNS blocklist, as doing so will also disrupt the adversary's exfiltration destination.  

- Organizations using Microsoft 365 should consult Microsoft's instructions for responding to a compromised email account and implement multi-factor authentication as part of their remediation strategy.

## Risks 

- Unintentionally removing non-malicious mailbox rules may disrupt business operations. 

- Failing to sever the adversary's access to the account will only temporarily set back the adversary and permit the adversary to reintroduce malicious forwarding rules.  

## Guidance 

Adversaries have multiple ways of creating malicious email rules. These include abusing email client software (e.g., Microsoft Outlook or Mozilla ThunderBird), web applications (e.g., Outlook on the web / OWA), and command-line interfaces (e.g., PowerShell).  

Adversaries may also hide their rules so they are not visible in Outlook or Exchange Administration tools by abusing the Microsoft Messaging API (MAPI) to modify the rule properties. Both visible and hidden malicious forwarding rules must be identified and removed.  

Email administrators should also consider disabling the ability for users to forward emails outside of the domain all together. 

When investigating a suspicious forwarding address or rule, administrators should consider using Message Trace to understand the flow of emails sent by a suspicious forwarding rule.  

### Mailbox Forwarding Rules 

#### Audit for Suspicious Forwarding Addresses 

Adversaries configure the user's mailbox settings to forward mail to a designated mailbox address. By specifying the attribute ForwardingSmtpAddress with the cmdlet Get-Mailbox, adversaries can forward mail to a mailbox destination outside of the organization. Likewise, by using the attribute ForwardingAddress, adversaries can forward emails to an internal destination.  

To identify suspicious mailbox forwarding rules, administrators should list and review all mailboxes configured with forwarding Address from Mailbox Settings. This can be accomplished with the Get-Mailbox cmdlet. Administrators can search Microsoft Unified Audit Logs using PowerShell through the ExchangeOnlineManagement module for past evidence of adversaries modifying mailbox settings. 

#### Remove Suspicious Forwarding Address 

To remove a suspicious forwarding address, administrators can use the PowerShell cmdlet Set-Mailbox and set ForwardingAddress and/or ForwardingSmtpAddress to $Null. 

#### Audit for Suspicious Inbox Forwarding Rules 

If an organization suspects a suspicious mailbox forwarding rule has been added or has identified  one or more malicious mailbox forwarding rules, performing an audit on all inbox forwarding rules will allow an organization to locate any overlooked forwarding rules introduced to one or more user accounts.  

To audit Microsoft Exchange logs,  administrators can log into Microsoft365 and search the audit log for suspicious use of the following cmdlets. 

-   New-InboxRule  
-   Set-InboxRules 
-   UpdateInboxRules 

When retrieving inbox rules, use the flag -includehidden to view hidden rules as well.  

#### Remove Mailbox Forwarding Rule 

If a malicious mailbox forwarding rule is known, administrators can remove it using the command line with PowerShell in  Microsoft365. This can be accomplished with the Remove-InboxRule cmdlet and associated rule ID.

### Mail Flow Rules 

#### Audit for Suspicious Mail Flow Rules 

Administrators can set up email forwarding rules that apply across the entire organization, rather than just individual inboxes. This is useful for organizations using platforms like Microsoft Exchange, which allow mail flow rules (also called transport rules) that automatically filter incoming emails based on specified conditions and perform actions on them. However, adversaries that abuse such features may be able to enable forwarding on all or specific mail an organization receives.

Following an incident, administrators should review logs for the creation of these rules. This can be accomplished with the Get-TransportRule cmdlet to view transport rules (mail flow rules). If a suspicious mail flow rule is identified, rotate the credentials for accounts belonging to email admins, ensure multi-factor authentication is deployed (ideally other than SMS), and investigate the admin account compromise. While investigating, the adversary's transport rule can be disabled with the cmdlt  Disable-TransportRule  using  ExchangePowerShell. 

#### Remove Suspicious Mail Flow Rule 

Mail flow rules can be removed with the ExchangePowerShell cmdlet Remove-TransportRule after specifying a Rule ID. 

### Block External Forwarding  

If necessary, email administrators can disable external forwarding all together by creating a new MailFlow rule. Senders should receive a Non-Delivery Report (NDR) with code 5.7.520 to tell them that the organization doesn't allow external forwarding. 

### Investigate Potential Methods of Persistence 

Even if a rule is removed and credentials rotated, adversaries may still have implemented automated processes to reestablish forwarding in the account.  

Incident responders should also triage the account owner's device(s) to identify whether malware has been granted the Mailbox.Settings.ReadWrite  permission. In cases where it is suspected that the user's device is compromised, reimaging may be needed to resolve the incident.  

Microsoft365 subscribers should audit all the audit logs for a compromised account, as there are often multiple means by which adversaries establish persistence. This includes Microsoft Flow rules or granting inappropriate mailbox permissions. 

Verify that the SMTP server is not compromised.   

## Associated Techniques 

-   [T1114.003](https://attack.mitre.org/techniques/T1114/003/) - Email Collection: Email Forwarding Rule 

## Related Countermeasures 

- CM0061 - Rotate Mailbox Passwords
- CM0074 - Audit and Restrict Mailbox Permissions 
- CM0076 - Remove Adversary Certificates and Rotate Secrets for Applications and Service Principals
- CM0080 - Block and Reissue Tokens

## References 

- Alert classification for suspicious inbox forwarding rules | <https://learn.microsoft.com/en-us/defender-xdr/alert-grading-playbook-inbox-forwarding-rules>
- Detecting suspicious email forwarding rules on Office 365 | <https://redcanary.com/blog/threat-detection/email-forwarding-rules/>
- Forward thinking: How adversaries abuse Office 365 email rules | <https://redcanary.com/blog/threat-detection/o365-email-rules-mindmap/>
- Threat Hunting in Microsoft 365 Environment | <https://www.youtube.com/watch?v=2A0faMIEp00>
