# Remove Suspicious Scheduled Tasks

* **ID:** CM0147
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine, Disable
* **Status:** Active
* **Nice Work Role:** Incident Response (PD-WRL-003)
* **Skills & Knowledge:** Windows administration; Task Scheduler auditing and malicious task removal

## Intended Outcome

Auditing Windows Task Scheduler prevents adversary attempts at persistence, execution, or privilege escalation via malicious tasks.

## Introduction

Windows Task Scheduler is a Microsoft Windows service that allows you to automate tasks by scheduling programs or scripts to run at specific times and/or intervals. Adversaries leverage this functionality to automate the execution of their own malicious code. This has been used to persist via execution on startup, escalate privilege by running the task under the context of a specific account, to mask code execution under the trusted Windows Task Scheduler process, or to achieve a variety of many other malicious goals an adversary may carry out via task execution. This countermeasure aims to provide guidance on auditing and removing of tasks that have been created or manipulated to fulfill adversary objectives.

## Preparation

- Identify valid baseline for tasks in the environment, including what each valid task does specifically.
- Ensure that task configurations are backed up before making any changes.

## Risks

- Modifying scheduled tasks may disrupt and impact previously automated system operations. 

## Guidance

### Viewing currently scheduled tasks

#### Audit Windows Tasks via Task Scheduler GUI

- Windows natively has the Task Scheduler system application. This can be used to view, add, delete, edit, or forcibly run scheduled tasks. 
- Use this application to audit currently set tasks and determine any abnormalities or malicious tasks. 

#### Audit Windows Tasks via command line

- The Schtasks command can be used to view, edit, or schedule tasks on windows. Similar to the Task Scheduler application, use this to audit for any abnormal or malicious tasks that may be present.
- In PowerShell, the Get-ScheduledTask module can also be used to query Scheduled Tasks

#### Audit Window Tasks via Autoruns

- Autoruns is a Windows utility apart of the Sysinternals package that shows which programs or tasks are scheduled to run on startup, login, or when you start other applications. 
- It is not natively installed, so it must be downloaded via Microsoft's website. 
- Autoruns requires no setup and can be used to audit programs scheduled to run on the device.

#### Detecting Hidden Tasks

- Adversaries can create tasks hidden from Schtasks and Task Scheduler by removing security descriptors from the tasks. Querying security descriptors of existing tasks and filtering for empty or unusable descriptors can detect adversaries using this method.

### Removing malicious tasks

#### Malicious task identification

- Review the output of Task Scheduler and examine each entry for legitimacy and validity.
- Analyze the commands, scripts, or applications executed by the tasks. Verify that there are no malicious or unrecognizable instructions within the tasks.
- Compare the tasks to a valid baseline configuration, look for any that do not belong or that have been modified from what is typical.

#### Removing tasks

- Tasks can be removed in the context menu of task scheduler, through the /delete of Schtasks, or also within Autoruns.
- Grab any important artifacts that could be useful for further Threat Hunting before deletion.

### Auditing changes to Task Scheduler

#### Enabling Logging of Task creation via Group Policy

Many attackers will sometimes have processes re-add Scheduled Tasks as a form of persistence. Windows logging can be enabled to log these task creations in order to stop adversaries. This can be enabled via group policy at 'Computer Configuration -> Windows Settings -> Security Settings -> Object Access -> Audit Other Object Access Events'. This will create event ID 4698 which will include some default traffic as well as malicious task creation.

## Associated Techniques

-   [T1053.005](https://attack.mitre.org/techniques/T1053/005/) - Scheduled Task/Job: Scheduled Task

## Related Countermeasures

- CM0023 - Identify and Terminate Suspicious Processes
- CM0030 - Remove Known Malware
- CM0038 - Remove Windows Management Instrumentation (WMI) Event Subscription
- CM0103 - Deny log on as a batch job
- CM0109 - Audit and Restrict Cron Daemon
- CM0120 - Remove Cron Job

## References

-   Task Scheduler Microsoft Docs | <https://learn.microsoft.com/en-us/windows/win32/taskschd/task-scheduler-start-page>
-   Schtasks Microsoft Docs | <https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/schtasks>
-   Sysinternals Autoruns | <https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns>
-   Using ScheduledTasks Module to audit tasks in PowerShell | <https://mcpmag.com/Articles/2017/04/27/ScheduledTasks-Module-to-Audit-Tasks-in-PowerShell.aspx?>
-   Diving into hidden scheduled tasks | <https://www.binarydefense.com/resources/blog/diving-into-hidden-scheduled-tasks/#:~:text=In%20the%20Microsoft%20post%20one%20of%20the%20pieces,tasks%20which%20do%20not%20have%20a%20SD%20value.>
-   Windows task auditing/logging | <https://www.csoonline.com/article/567041/how-to-audit-windows-task-scheduler-for-cyber-attack-activity.html>
