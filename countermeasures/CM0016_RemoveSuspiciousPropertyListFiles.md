# Remove Suspicious Property List (PLIST) Files

* **ID:** CM0016
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Eliminate
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** macOS administration; property list files and persistence basics

## Intended Outcome

Removing suspicious Property List (PLIST) files terminates adversary
defense evasion and persistence using modified PLIST files.

## Introduction

A PLIST (Property List) file is a file format used to store structured data on Apple
platforms, including macOS and iOS. Because these files are commonly
used for storing configuration settings and preferences in XML, JSON, or
binary, adversaries may modify or introduce weaponized PLIST files to
achieve persistence or manipulate them to execute malicious activities.

Malicious PLIST files can be stored in a number of directories
including:

-   \~/Library/LaunchAgents
-   \~/Library/LaunchAgent
-   \~/Library/LaunchDaemons
-   \~/Library/Application Support
-   \~/Library/Preferences
-   \~/Library/StartupItems

## Preparation

- Unhide the directories where malicious PLIST files are commonly stored.

## Risks

- This countermeasure can break legitimate functionality.
- Legitimate applications may store their configuration settings in a PLIST file that is under suspicion.
- This countermeasure may not stop all ongoing adversary activity.
- Adversaries may use multiple malicious PLIST files, and may have established other persistence mechanisms.

## Guidance

- Investigate Launch Agents and Launch Daemons as a means of achieving persistence on macOS.
- Quarantine the suspicious PLIST file via the organization’s anti-virus or endpoint detection and response. 
- Deploy tools to collect and analyze macOS data and artifacts (AutoMacTC, Aftermath, etc.)
- If a PLIST file appears modified to trigger malicious action, investigate the process that modified the PLIST file for malicious properties.
- Restrict unprivileged access to persistence directories (e.g., * ~/Library/LaunchAgents and ~/Library/LaunchDaemons) 

## Associated Techniques

- [T1647](https://attack.mitre.org/techniques/T1647) - Plist File Modification
- [T1543.001](https://attack.mitre.org/techniques/T1543/001) - Create or Modify System Process: Launch Agent
- [T1543.004](https://attack.mitre.org/techniques/T1543/004) - Create or Modify System Process: Launch Daemon

## Related Countermeasures

- CM0022 - Quarantine Suspicious Files
- CM0120 - Remove Cron Job

## References

- About Information Property List Files | <https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AboutInformationPropertyListFiles.html>
- How Malware Persists on macOS | <https://www.sentinelone.com/blog/how-malware-persists-on-macos/>
