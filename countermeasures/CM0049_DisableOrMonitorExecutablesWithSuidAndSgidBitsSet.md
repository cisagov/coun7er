# Disable or Monitor Executables with SUID and SGID Bits Set

* **ID:** CM0049
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Linux/Unix administration; SUID/SGID permissions and executable monitoring

## Intended Outcome

Disabling or monitoring executables with SUID and SGID bits set prevents or detects adversaries from performing privilege escalation using misconfigured application permissions. 

## Introduction

Set User ID (SUID) and Set Group ID (SGID) enables an executable on Linux to run with the permissions of the owner. 

## Preparation

No Preparation content identified. 

## Risks

- Executables that requires elevated privileges will not work for non-privileged users. 
- Tools that depend on the ability to execute SUID/SGID binaries may be disrupted. 

## Guidance

### Disabling SUID & SGID Executables

To disable executables with setuid or setgid permissions, use sudo chmod u-s or chmod g-s respectively. This will remove the designated executable set permissions. If the executables with permissions set are not known, `find / -perm -u=s -type f 2>/dev/null` can be used to find all binaries with SUID starting at the root directory. SGID binaries can be discovered using the same command, but, replace `-u=s` with `-g=s`. Furthermore, the security-misc package my kick-secure can be installed and configured to automate the process of discovering and disabling suid binaries and libraries. A preventative measure that can be used is to mount a filesystem with nosuid flag set. This will prevent the use of executables with SUID or SGID set at or within those locations. 

### Monitoring SUID & SGID Binaries

Execution of SUID and SGID binaries are not logged by default. If command history was not cleared, it is possible to check if an SUID/SGID binary was executed or if attempts to discover SUID/SGID binaries were made. The auditd package can be installed and configured to log execution of files and binary discovery commands by configuring the audit rules. Moreover, ps aux can be used to check what processes are running and by whose authority. 

## Associated Techniques

- [T1548.001](https://attack.mitre.org/techniques/T1548/001/) - Abuse Elevation and Control: Setuid and Setgid

## Related Countermeasures

- CM0023 - Identify and Terminate Suspicious Processes
- CM0067 - Install Critical Software Security Updates
- CM0109 - Audit and Restrict Cron Daemon

## References

- Linux 101: What is the SUID Permission? | <https://www.techrepublic.com/article/linux-101-what-is-the-suid-permission/>
- SUID Disabler and Permission Hardener | <https://www.kicksecure.com/wiki/SUID_Disabler_and_Permission_Hardener>
- security-misc: Enhance Miscellaneous Security Settings | <https://www.kicksecure.com/wiki/Security-misc>
- Linux File Permissions: Understanding setuid, setgid, and the Sticky Bit | <https://www.cbtnuggets.com/blog/technology/system-admin/linux-file-permissions-understanding-setuid-setgid-and-the-sticky-bit>
- Setuid and Setgid | <https://redcanary.com/threat-detection-report/techniques/setuid-setgid/>
- Configure Linux system auditing with auditd | <https://www.redhat.com/en/blog/configure-linux-auditing-auditd>
- The Pwnkit Vulnerability: Overview, detection, and remediation | <https://www.datadoghq.com/blog/pwnkit-vulnerability-overview-and-remediation/>
