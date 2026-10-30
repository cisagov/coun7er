# Implement SPF, DKIM, and DMARC Email Authentication Methods

* **ID:** CM0055
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Network Operations (IO-WRL-004)
* **Skills & Knowledge:** Email/DNS administration; SPF, DKIM, and DMARC

## Intended Outcome

Enabling Sender Policy Framework (SPF), DomainKeys Identified Mail (DKIM), and Domain-based Message Authentication Reporting and Conformance (DMARC) email authentication methods restricts adversary initial access via email sent on behalf of a domain they do not own. 

## Introduction

Sender Policy Framework (SPF) is an authentication protocol that lists IP addresses in a DNS TXT record that are authorized to send email on behalf of domains.  A sent email message will be checked for an SPF record and checked against an authorized sender list.  If the valid SPF record exists, a policy of “PASS” or “FAIL” will be applied.  Failed policy application should result in the message being routed to a spam folder.   

DomainKeys Identified Mail (DKIM) adds a digital signature to email headers to ensure email content has not changed.  A sent email message will include a private key to encrypt the email signature.  The public key is included in a DNS TXT record.  The message recipient validates the signature by comparing the public/private keys.    

Domain-based Message Authentication, Reporting, and Conformance (DMARC) is an email authentication protocol that uses SPF and DKIM to validate the sender and block fraudulent messaging.  It enables a domain owner to specify a policy for handling unauthenticated messages.    

These tools ensure that an email message is genuine and is coming from a validated source.  These protocols are not intended to work in isolation and should be implemented together.  Configuring these protocols differs between domain hosting providers.  The guidance herein is intended to describe the process common to all.      

## Preparation

- Identify DNS provider and ensure access to the management console.
- Identify all email senders (services/servers).
- Some service providers implement these protocols by default.  Check the status of implementation with the organization’s email provider. 

## Risks

Incorrectly configuring SPF, DKIM, and DMARC can result in legitimate email messages being marked as spam.  To mitigate this risk, validate SPF, DKIM, and DMARC records and policies prior to deployment.  Continually monitor and refine policy implementation.  

## Guidance

This guidance assumes the organization is using a domain provider.  To implement this guidance, login to the domain provider’s management console.  References to specific domain provider guidance is provided below.  This list is not exhaustive, please refer to the organization’s domain provider for tailored guidance.  

### SPF

1.	Define your SPF record for your domain provider.  Ensure syntax is in accordance with domain provider guidance.  
    - If the organization sends mail with additional services/servers, create a custom SPF record to authorize these services/servers.
2.	Add the SPF record at the domain provider’s management console.
    - If necessary, apply the SPF record to subdomains.
3.	Specify the enforcement rule to apply to the SPF record.  The “all” rule is recommended. 
4.	Verify the SPF records for all relevant domains/subdomains in the domain provider’s management console.

### DKIM

1.	Generate the public key.
2.	Create and add a DNS TXT record to the organization’s DNS manager.  
3.	Check the validity of the DKIM implementation and enable email signing. 

### DMARC

1.	Ensure the existence of SPF and DKIM.  If these protocols are not currently being implemented, they will need to be configured. 
2.	Generate a DMARC record. 
3.	Add the DMARC record to DNS for each domain.  DMARC policies can be specified for subdomains.  If policies are not specifically applied to subdomains, they will inherent the parent domain’s DMARC policy.  
4.	Modify the policy accordingly. 

## Associated Techniques

- [T1566.001](https://attack.mitre.org/techniques/T1566/001/) – Phishing: Spearphishing Attachment
- [T1566.002](https://attack.mitre.org/techniques/T1566/002/) – Phishing: Spearphishing Link
- [T1566.003](https://attack.mitre.org/techniques/T1566/003/) – Phishing: Spearphishing via Service

## Related Countermeasures

No Related Countermeasures content identified.

## References

### SPF Guidance 

- Help prevent spoofing and spam with SPF | <https://support.google.com/a/answer/33786>
- Set up SPF to identify valid email sources for your Microsoft 365 domain | <https://learn.microsoft.com/en-us/defender-office-365/email-authentication-spf-configure?view=o365-worldwide>
- Video: How to add an SPF record for a domain in your Namecheap account | <https://www.namecheap.com/support/knowledgebase/article.aspx/317/2237/how-do-i-add-txtspfdkimdmarc-records-for-my-domain/>
- Add an SPF record | <https://www.godaddy.com/help/add-an-spf-record-19218>
- Using a custom MAIL FROM domain | <https://docs.aws.amazon.com/ses/latest/dg/mail-from.html>

### DKIM Guidance 

- Turn on DKIM for your domain | <https://support.google.com/a/answer/174124>
- Set up DKIM to sign mail from your Microsoft 365 domain | <https://learn.microsoft.com/en-us/microsoft-365/security/office-365-security/email-authentication-dkim-configure?view=o365-worldwide>
- How do I add TXT/SPF/DKIM/DMARC records for my domain? | <https://www.namecheap.com/support/knowledgebase/article.aspx/317/2237/how-do-i-add-txtspfdkimdmarc-records-for-my-domain/>
- Enable and add DKIM to my domain for Microsoft 365 | <https://www.godaddy.com/help/enable-and-add-dkim-to-my-domain-for-microsoft-365-41748>
- Easy DKIM in Amazon SES | <https://docs.aws.amazon.com/ses/latest/dg/send-email-authentication-dkim-easy.html>

### DMARC Guidance 

- Add your DMARC record | <https://support.google.com/a/answer/2466580>
- Set up DMARC to validate the From address domain for senders in Microsoft 365 | <https://learn.microsoft.com/en-us/microsoft-365/security/office-365-security/email-authentication-dmarc-configure?view=o365-worldwide>
- How do I add TXT/SPF/DKIM/DMARC records for my domain? | <https://www.namecheap.com/support/knowledgebase/article.aspx/317/2237/how-do-i-add-txtspfdkimdmarc-records-for-my-domain/>
- Set up SPF, DKIM, or DMARC records for my hosting email | <https://www.godaddy.com/help/set-up-spf-dkim-or-dmarc-records-for-my-hosting-email-40810>

### Verification Tools 

- DMARC deployment Tools | <https://dmarcly.com/tools/>
- DKIM, SPF, DMARC DNS Verification Tool | <https://www.activecampaign.com/dkim-spf-check/>
- DMARC Email Delivery Tools | <https://mxtoolbox.com/dmarc/dmarc-email-tools?referrer=cms_dmarchome>
