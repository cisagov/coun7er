# Block Default Screensaver Programs and Monitor SCR Executable Files

* **ID:** CM0015
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; executable controls and file monitoring

## Intended Outcome

Blocking default screensaver programs and monitoring SCR executable
files restricts and detects adversary privilege escalation and
persistence using malicious SCR files.

## Introduction

No Introduction content identified.

## Preparation

No Preparation content identified.

## Risks

No Risks content identified.

## Guidance

### Monitor

Configure Endpoint Detection and Response (EDR) tools to alert for execution of suspicious .scr files. 

- Monitor .scr files executed from non-standard locations (i.e., locations outside of C:\Windows\System32\\, C:\Windows\sysWOW64\\, C:\Windows\servicing\LCU\\, and C:\Windows\WinSxS\\)
- Monitor .scr files associated with unexpected modifications to registry hives (e.g., HKCU:\Control\Panel\Desktop\\)
- Monitor .scr files with nonstandard names (e.g., Bubbles.scr, Mystify.scr, scrnsave.scr).

### Block

- Block .scr Files at gateways
- Use Group Policy Objects (GPOs) to disable default screensavers
    - Disable the following: 
        - Enable screen saver
        - Screen saver timeout
        - Force specific screen saver
    - Enable the following:
        - Prevent changing screen saver

## Associated Techniques

- [T1546](https://attack.mitre.org/techniques/T1546) - Event Triggered Execution
- [T1546.002](https://attack.mitre.org/techniques/T1546/002) - Event Triggered Execution: Screensaver

## Related Countermeasures

- CM0013 - Block Windows Management Instrumentation (WMI) Service
- CM0138 - Audit and Remove Malicious Udev Rules

## References

- Persistence - Screensaver | <https://pentestlab.blog/2019/10/09/persistence-screensaver/>
- Force Disable Screen Saver in Windows 10 | <https://winaero.com/force-disable-screen-saver-in-windows-10/>
- Analysis of malicious Screen Saver files | <https://medium.com/@APT-410/analysis-of-malicious-screen-saver-files-adb3b7c01939>
