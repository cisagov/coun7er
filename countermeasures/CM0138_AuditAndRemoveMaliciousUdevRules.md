# Audit and Remove Malicious Udev Rules

* **ID:** CM0138
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine, Eliminate
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Linux administration; udev rule auditing and persistence removal

## Intended Outcome

Auditing and removing malicious Udev rules detects and eliminates an adversary's attempts to achieve execution and persistence via malicious udev rules. 

## Introduction

Udev replaced the Device File System (devfs) in Linux kernel versions 2.6 and above, allowing the kernel or users to identify devices based on properties, such as vendor and device ID. Udev runs in the userspace, as opposed to the kernel space like devfs. The system is comprised of the udevd daemon and kernel services: the kernel alerts the daemon of certain events happening in the userspace and the daemon responds to the events with configured actions, called rules. Notably, Udev offers the system mechanisms to identify change in device state, such as when it is plugged in or removed. Administrators can create rules to rename network interfaces or modify user rights when a hard disk is plugged in.

Udev is a sandbox environment by default, so long-running processes, daemons, or network connections are not allowed within Udev rules. However, a skilled adversary can bypass the sandbox restrictions via `at` or process injection. 

Adversaries can create malicious rules to execute scripts whenever certain conditions are met. For example, by configuring a script to run every time `/dev/random/` (corresponds to device minor number 8 ) is loaded, adversaries get their code to run on a system after every restart, as `/dev/random/` loads upon reboot.   

## Preparation

- Take inventory of all Linux systems that may have the Udev utility. 
- Ensure appropriate permissions to view and edit udev rules. 
- Ensure that Udev rule configurations are backed up or saved before making changes in case they need to be reverted. 
- Ensure process logging is enabled in the environment. 

## Risks

- Removing a legitimate Udev rule that is used in the environment may break functionality or impact operations. Ensure that rules are not in use or not legitimate before removing. 

## Guidance

### Audit Udev Rules

- Review udev rules located in `/etc/udev/rules.d/`, `/run/udev/rules.d/`, or `/usr/lib/udev/rules.d/`. 
- Note any rules that contain suspiciously named scripts in the `RUN` attribute, or scripts with known malicious names. The rule may contain an absolute or relative path to the scripts: ensure that the script is benign or remove both the file and rule.
- Note any rules that run "on restart" such as rules regarding `/dev/random/`, any USB attachment, or non-loopback network connections, since adversaries may be using these rules to maintain persistence in the environment. Some of the identified rules may be legitimate.
- Note any rules containing `at` or other task schedulers in their `RUN` attribute, or other attempts to escape the sandbox environment to maintain persistence. 

### Remove Unnecessary Rules

- Delete the identified .rules file via `rm`. Ensure the file is not needed before deleting. 
- Reload Udev with the `udevtrigger` command.

### Monitor Udev Rule Creation

- Monitor creation or modification of files in the `/etc/udev/rules.d/`, `/run/udev/rules.d/`, or `/usr/lib/udev/rules.d/` directories. Take specific note of rules containing the `at` utility or other signs of process injection. 
- Monitor process creation for children of the `system-udevd.service` process. 

## Associated Techniques

- [T1546](https://attack.mitre.org/techniques/T1546/) - Event Triggered Execution

## Related Countermeasures

- CM0045 - Disable or Monitor Execution of WMIC.exe
- CM0015 - Block Default Screensaver Programs and Monitor SCR Executable Files
- CM0067 - Install Critical Software Security Updates
- CM0038 - Remove Windows Management Instrumentation (WMI) Event Subscription
- CM0109 - Audit and Restrict Cron Daemon
- CM0120 - Remove Cron Job
- RM051? - Audit Windows Task Scheduler (no number at the moment)

## References

- udev | <https://wiki.debian.org/udev>
- New Linux Malware 'sedexp' Hides Credit Card Skimmers Using Udev Rules | <https://thehackernews.com/2024/08/new-linux-malware-sedexp-hides-credit.html>
- Leveraging Linux udev for persistence | <https://ch4ik0.github.io/en/posts/leveraging-Linux-udev-for-persistence/>