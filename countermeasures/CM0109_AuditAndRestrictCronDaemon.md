# Audit and Restrict Cron Daemon

* **ID:** CM0109
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Linux administration; cron auditing and scheduled-job access controls

## Intended Outcome

Auditing and restricting the use of the Linux Cron daemon prevents adversary persistence and privilege escalation capabilities.

## Introduction

The cron daemon is a time-based job scheduler in Unix-like operating systems, including Linux. It is commonly used for automating system maintenance, running scripts, and executing commands at scheduled intervals. However, adversaries can abuse the cron service to establish persistence, escalate privileges, and execute malicious payloads. This countermeasure aims to provide comprehensive guidance on auditing and restricting the use of the cron daemon to mitigate these risks.

## Preparation

- Take inventory of all Linux systems and note the presence of the cron utility.
- Document all cron jobs currently scheduled on enterprise systems. Identify the user accounts associated with existing cron jobs and the scripts or commands being executed.
- Ensure that the cron job configurations are backed up before making any changes.

## Risks

- Modifying cron job configurations may disrupt scheduled tasks and impact previously automated system operations. 
- Failure to restrict cron jobs by following the principle of least privilege creates system vulnerabilities that enable potential execution and persistence objectives by an adversary.

## Guidance

### Auditing Cron Jobs

- Utilize the following commands to list cron jobs for all users:
	- `crontab -l`
	- `sudo cat /etc/crontab `
	- `sudo ls -la /etc/cron.d/`
- Review the output of these commands, examining each entry for validity and legitimacy of scheduled tasks.
- Analyze the scripts and commands executed by cron jobs. Ensure they do not contain malicious or unnecessary instructions. The behavior of the cron jobs should be cross referenced with the ownership and permissions of the scripts executed by them.
- Implement file integrity monitoring (FIM) tools to track changes to common cron-related files such as `/etc/crontab`, `/etc/cron.d/`, and `/var/spool/cron/`).
- Set up alerts for unauthorized changes to cron job configurations.
- It is important to note that adversaries will often delete compromised or malicious cronjobs after they have fulfilled their purpose- monitor the system logs (`/var/log/syslog and/or /var/log/cron`) for cases of cronjob deletion.
- Implementing organized logging of crontabs and their related jobs. First, ensure that cron job executions are being properly logged under the syslog utility.
	- Example - Capture Cron Job Execution Logs:
		 `sudo echo "cron.* /var/log/cron.log" | sudo tee -a /etc/rsyslog.d/50-default.conf` 
	     `sudo systemctl restart rsyslog`
- Instead of defaulting logged output of cronjobs to `/var/log/syslog`, best practices suggest that the output of jobs be logged in separate files based on levels of criticality.

### Restricting Cron Jobs

- Restrict the use of cron jobs to essential services and administrative tasks.
- Disable cron job scheduling for regular users unless absolutely necessary.
- Ensure that only authorized users have the necessary permissions to edit and create cron jobs. The following commands exemplify configuration of cron permissions using the `chmod` command:
  `sudo chown root:root /etc/crontab` 
  `sudo chmod 600 /etc/crontab` 
  `sudo chown root:root /etc/cron.d/` 
  `sudo chmod 700 /etc/cron.d/`
- Additionally, the `cron.allow` and `cron.deny` files should be properly configured so as to limit cron execution under the principle of least privilege.    
    -Create `/etc/cron.allow` to specify users allowed to use cron.
    -Create `/etc/cron.deny` to specify users denied from using cron.
    Example:
        `echo "admin_user" | sudo tee -a /etc/cron.allow` 
        `echo "untrusted_user" | sudo tee -a /etc/cron.deny`
- Restrict access to the crontab directory to prevent unauthorized users from scheduling cron jobs, ensuring regular review of the contents of the `/var/spool/cron/` directory for unauthorized entries.

## Associated Techniques

- [T1053.003](https://attack.mitre.org/techniques/T1053/003/) - Scheduled Task/Job: Cron

## Related Countermeasures

- CM0049 - Disable or Monitor Executables with SUID and SGID Bits Set
- CM0068 - Audit and Restrict Exchange ApplicationImpersonation Role
- CM0036 - Identify and Monitor Remote Access Tools
- CM0103 - Deny Logon as a Batch Job (Future)
- CM0103 - Deny log on as a batch job
- CM0120 - Remove Cron Job
- CM0138 - Audit and Remove Malicious Udev Rules
- CM0147 - Remove Suspicious Scheduled Tasks

## References

- 15 Restricting cron and at | <https://documentation.suse.com/sles/15-SP3/html/SLES-all/cha-sec-cron-at.html>

- How to Effectively Monitor and Manage Cron Jobs | <https://collabnix.com/how-to-effectively-monitor-and-manage-cron-jobs/>
