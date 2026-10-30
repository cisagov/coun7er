# Investigate Account Manipulation

* **ID:** CM0064
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active
* **Nice Work Role:** Incident Response (PD-WRL-003)
* **Skills & Knowledge:** Incident response; identity logs and account-change analysis

## Intended Outcome 

Investigating account manipulation detects and terminates adversary persistence and privilege escalation.

## Introduction 

Account manipulation efforts by an adversary is often evidenced by credential changes or unusual authentication traffic. The unauthorized modification of accounts allows an adversary to maintain access and escalate privileges within a compromised system. Investigation of credential change events is necessary when logs or network traffic evidence the manipulation of user accounts, passwords, and permissions that may enable an adversaries' persistent access of one or more accounts. 

Effective detection and investigation of unauthorized credential changes are vital to disrupting adversarial persistence capabilities and mitigating further damage to organizational resources via privilege escalation.

## Preparation 

- If not present, establish a comprehensive baseline of legitimate user account details, including history of legitimate user account credentials, access patterns, and expected behaviors.
- Ensure a process or system for managing account creation and modification exists within the domain. Ensure technical policies are in place that temper the usage of authorized access and privileged accounts.
- Ensure monitoring solutions such as EDRs and SIEMs are configured to properly alert on credential change or account manipulation events. Tailor monitoring towards out of cycle credential changes, failed login attempts, and unusual resource access.

## Risks 

- Full-scale investigation of authentication and account manipulation events introduces overhead to domain administrators on events that can otherwise be false positives. Configure monitoring solutions to exclude policy-regulated credential changes or account access events that fall within expected time constraints.

## Guidance 

### Forensic Analysis

- Utilize forensic techniques such as open source intelligence to determine the origin of the credential access or logon event by pinpointing the IP address and type of device utilized.
- Cross reference the history and use case of the modified credentials to determine if a high frequency of modification is expected for the credentials.
- Isolate the affected system from the network after coming to a forensic conclusion.

### Enforce Credential Policy

- If credential changes or access activity is deemed to be unauthorized, immediately initiate a password reset on the affected account.
- After each credential compromise event, it is recommended to increase password complexity and implement multi-factor authentication as part of organizational policy.
- Conduct regular review and update of security policies related to credential management.

### Implement or Configure Monitoring

- Deploy real-time monitoring solutions such as Security Information and Event Management (SIEM) or Endpoint Detection Response (EDR) solutions to enable log-based analysis of credential modification events.
- Ensure credential change and authentication events are routed through IAM systems that enforce policy compliance and logging of administrative actions.

## Associated Techniques 

- [T1098](https://attack.mitre.org/techniques/T1098/) - Account Manipulation

## Related Countermeasures

- CM0063 - Investigate Suspicious Login Attempts
- RM0293 - Investigate Failed Logins after Credential Change (*future*)
- CM0034 - Identify and Block Suspicious Hosts
- CM0112 - Remove Extraneous and Stale Accounts
- CM0118 - Revoke Existing Administrator Permissions and Deploy New Administrator Accounts
- CM0136 - Configure and Invalidate Relative ID (RID) Pools
- CM0140 - Configure Windows Audit Policy

## References 

- Comprehensive Guide for Compromised Credential Detection | <https://www.silverfort.com/blog/detecting-compromised-credentials/>
- Detecting Post-Compromise Threat Activity in Cloud Environments | <https://www.cisa.gov/news-events/cybersecurity-advisories/aa21-008a>
