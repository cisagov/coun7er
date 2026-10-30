# Remove Cron Job

* **ID:** CM0120
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Eliminate
* **Status:** Active
* **Nice Work Role:** Incident Response (PD-WRL-003)
* **Skills & Knowledge:** Linux administration; crontab review and malicious scheduled-job removal

## Intended Outcome

Removing a malicious or manipulated Cron job prevents adversarial objectives of execution, persistence, and privilege escalation.

## Introduction

The cron daemon is a time-based job scheduler in Unix-like operating systems, including Linux. It is commonly used for automating system maintenance, running scripts, and executing commands at scheduled intervals. However, adversaries can abuse the cron service to establish persistence, escalate privileges, and execute malicious payloads. This countermeasure aims to inform the process of removing a cron job that has been created or manipulated to fulfill an adversary's campaign.

## Preparation

- Take inventory of all Linux systems and note the presence of the Cron utility.
- Document all Cron jobs currently scheduled on enterprise systems. Identify the user accounts associated with existing Cron jobs and the scripts or commands being executed.
- Ensure that the Cron job configurations are backed up before making any changes.

## Risks

- Modifying Cron job configurations may disrupt scheduled tasks and impact previously automated system operations. 
- Failure to restrict Cron jobs by following the principle of least privilege creates system vulnerabilities that enable potential execution and persistence objectives by an adversary.

## Guidance

### Removing Malicious Cron Jobs

- Identify suspicious or unauthorized cron jobs by cross-referencing with documented and approved Cron jobs.
- Remove the Cron jobs that are identified to be malicious or manipulated using the following commands:
	- To remove Cron jobs that run at a user level `crontab -r -u <username>`
	- To remove system-wide/universal Cron jobs:
		- `sudo rm /etc/cron.d/<malicious_cron_jobs>`
		- `sudo sed -i '/<malicious_cron_job>/d' /etc/crontab`
- Verify Cron jobs have been removed by listing Cron jobs and reviewing crontabs:
	- `ls /var/spool/cron/crontabs`
	- List Cron jobs for all users:
		- `crontab -l`
		- `sudo cat /etc/crontab`
		- `sudo ls -la /etc/cron.d/`

## Associated Techniques

- [T1053.003](https://attack.mitre.org/techniques/T1053/003/) - Scheduled Task/Job: Cron

## Related Countermeasures

- CM0016 - Remove Suspicious Property List (PLIST) Files
- CM0030 - Remove Known Malware
- CM0038 - Remove Windows Management Instrumentation (WMI) Event Subscription
- CM0103 - Deny log on as a batch job
- CM0109 - Audit and Restrict Cron Daemon
- CM0138 - Audit and Remove Malicious Udev Rules
- CM0147 - Remove Suspicious Scheduled Tasks


## References

- Removing crontab Files | <https://docs.oracle.com/cd/E18752_01/html/817-0403/sysrescron-17.html>
