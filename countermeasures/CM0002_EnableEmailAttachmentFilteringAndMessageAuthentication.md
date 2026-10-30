# Enable Email Attachment Filtering and Message Authentication 

* **ID:** CM0002
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Infrastructure Support (PD-WRL-004)
* **Skills & Knowledge:** Email administration; attachment filtering; SPF/DKIM/DMARC basics

## Intended Outcome 

Enabling email attachment filtering and enabling message authentication restricts adversary initial access using malicious files. 

## Introduction 

An email gateway can filter incoming and outgoing email messages based on specified parameters. Email gateways provide a high level of control over what emails are to be filtered out and how. The attachment scanning capability commonly available with email gateways allows organizations to automatically scan email attachments and perform different tasks on them (hash checking, filetype checking, etc.) and then filter them out based on different criteria. Examples of file types typically blocked: .com, .exe, .dll, .ps1, .ps1xml, .msh, .cmd, .bat, .hta, .jar, .ws, .wsc, .rar, .bz2, .gz, .tar, .msi, .msu, .tmp, .iso, .img, .xls, .xlt, .xlm 

Sender Policy Framework (SPF) serves as an email authentication mechanism that verifies the IP address of the sending server matches the domain name of the sender’s email address. Implementing SPF helps to prevent spoofing attacks. Spoofing attacks can increase the likelihood of an authentic looking attachment with a malicious payload reaching an employee’s inbox. 

Domain Keys Identified Mail (DKIM) provides an additional layer of email authentication that helps to confirm the integrity of messages via digital signatures and can help prevent email tampering that might occur in transit from one location to another. 

Domain-based Message Authentication, Reporting, and Conformance (DMARC) is an email authentication protocol that allows control over how an organization’s emails should be handled should they fail SPF or DKIM checks, which can help to prevent email spoofing. DMARC can also generate reports on other useful email authentication measures such as Certificate Authorities Authorization (CAA), Authenticated Received Chain (ARC), and the Domain Name System-Based Authentication of Named Entities (DANE). 

## Preparation 

- Deploy and configure an email gateway. 
- Compile an up-to-date list of suitable or unsuitable file types that you wish the email gateway to either block or allow. 
- Ensure that the appropriate allowlist/denylist that best suits the organization’s business needs and security requirements are configured and up to date.
- Ensure that email file attachment hashes are being compared to a reliable and reputable database of known malicious hashes and that any files with a matching hash from the database are blocked. This can be performed either via the email gateway, or a third-party solution that is properly integrated with the email gateway. However, security teams should understand that just because a file’s hash doesn’t match a previously identified signature, this does not mean the file is verified to be safe as packing and obfuscating malicious files is comparatively easy for threat actors.

## Risks 

- This countermeasure can break legitimate functionality. 
- A phased implementation can reveal potential issues involving improper quarantining or blocking. During the first phase, e-mails to be quarantined are flagged and monitored, allowing issues to be identified and resolved. E-mails are quarantined in a subsequent phase. 

## Guidance 

### Email Gateway Configuration 

- Configure an email gateway to filter incoming and outgoing email messages based on specific parameters. 
- Enable the attachment scanning capability to automatically scan email attachments (hash checking, filetype checking, etc.) and filter them based on specified criteria.  If this capability does not exist on your solution, consider supplementing with antivirus integration. 

### Anti-Spoofing and Email Authentication Mechanisms 

- Implement Sender Policy Framework (SPF) to help prevent spoofing attacks.  Ensure emails with attachments that fail SPF checks are rejected, quarantined, or flagged as suspicious. 
- Ensure email services support Domain Keys Identified Mail (DKIM).  Configure DKIM settings for action if an email fails to pass a DKIM check.   
- Verify that your email services support Domain-based Message Authentication, Reporting, and Conformance (DMARC) and configure DMARC settings to specify the actions to be taken if an email fails to pass a check. 

### User/Employee Training 

- Provide education and training to inform users of the risks and best practices associated with email attachments. 

## Associated Techniques 

- [T1566.001](https://attack.mitre.org/techniques/T1566/001/) - Phishing: Spearphishing Attachment 

## Related Countermeasures 

- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0047 - Block High-risk File Types at Web Gateway
- CM0023 - Identify and Terminate Suspicious Processes
- CM0037 - Enable Geolocation-based Traffic Filtering 
- CM0113 - Disable Microsoft Office Add-ins

## References 

- Filtering and blocking email attachments using Trend Micro's Messaging products | <https://success.trendmicro.com/en-US/solution/KA-0003827>
- Defense Evasion and Phishing Emails | <https://redcanary.com/blog/defense-evasion-and-phishing-emails/>
- Spearphishing Attachment | <https://redcanary.com/threat-detection-report/techniques/spearphishing-attachment/>
- Mail flow best practices for Exchange Online, Microsoft 365, and Office 365 (overview) | <https://learn.microsoft.com/en-us/exchange/mail-flow-best-practices/mail-flow-best-practices/>
- Anti-spoofing protection in EOP | <https://learn.microsoft.com/en-us/microsoft-365/security/office-365-security/anti-phishing-protection-spoofing-about/>
- Strategies to Mitigate Cyber Security Incidents | <https://www.cyber.gov.au/resources-business-and-government/essential-cyber-security/strategies-mitigate-cyber-security-incidents/strategies-mitigate-cyber-security-incidents>
- Configure trusted ARC sealers | <https://learn.microsoft.com/en-us/microsoft-365/security/office-365-security/use-arc-exceptions-to-mark-trusted-arc-senders/>
- What is Certification Authority Authorization? | <https://pkic.org/2013/09/25/what-is-certification-authority-authorization/>
- Phishing | <https://www.nist.gov/itl/smallbusinesscyber/guidance-topic/phishing>