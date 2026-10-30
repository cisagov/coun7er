# Audit and Restrict PsExec

* **ID:** CM0141
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine, Disable
* **Status:** Active
* **Nice Work Role:** Infrastructure Support (PD-WRL-004)
* **Skills & Knowledge:** Windows administration; PsExec auditing and remote execution restrictions

## Intended Outcome

Auditing and restricting PsExec blocks or prevents an adversary's attempts at persistence, lateral movement, exploitation, and defense evasion by abusing PsExec.

## Introduction

PsExec is a legitimate Windows utility included as part of Sysinternals. It allows It allows users to achieve execution on remote systems without installing software on the client machine and is a lightweight replacement for Telnet. Administrators use PsExec to access remote systems via the Server Message Block (SMB) protocol. The process establishes an SMB connection, pushes a copy of PSEXECSVC.exe to the client, and launces the process, creating a named pipe (typically named `\\.\pipe\psexesvc`). 

Adversaries commonly use PsExec to move laterally in an environment, either by launching an interactive terminal or command prompt interface, or by providing PsExec with commands to execute remotely and then terminate. They may also use PsExec to transfer malicious files and further compromise the environment. To cover their tracks, adversaries may rename the dropped PsExecSVC.exe binary with the `-r` flag, but the internal binary name and the default named pipe remain. 

## Preparation

- Ensure process and event logging is configured. 
- Identify if and how PsExec is used in the environment. 

## Risks

- If PsExec is used in the environment, disabling it may be disruptive. Consider restricting and monitoring PsExec, or moving to PowerShell Remoting for similar but more secure functionality. 
- Renaming PsExec or using one of its copies or variants may avoid some of the detection. Consider restricting all similar binaries or setting up alerts for well-known PsExec replacements (such as PAExec). 

## Guidance

If PsExec is not necessary in the environment, disable and remove it. Otherwise, restrict execution to only necessary users and set up monitoring.

### Restrict PsExec

- Implement Attack Surface Reduction (ASR) rules to block execution of PsExec.
	- In Microsoft Defender Security Portal, navigate to `Device Control > Attack Surface Reduction`. 
	- Select `Edit` and navigate to the `Block process creations originating from PSExec and WMI commands` rule.
	- Enable the rule. Save and exit. 
- Restrict access to only specific users that may need PsExec for their roles, if other options (such as PowerShell Remoting) are unavailable. 
- Edit firewall rules to block PsExec to and from remote devices. 

### Monitor for PsExec

- Check any anti-virus alerts regarding PsTools and PsExec in particular.
- Monitor all calls to `psexec` from the command line. 
- Monitor named pipes such as `\\.\pipe\psexesvc` or similar. Other lateral movement tools such as MetaSploit also use named pipes, but so do legitimate services. 
- Monitor for binaries with internal names `PsExec` and `PsExec Service Host`. 
- If PsExec is not typically used in the environment, the registry key at `HKEY_CURRENT_USER\software\sysinternals\psexec\eulaaccepted` can indicate if it was run recently. In environments where PsExec is used legitimately, this may not be helpful. 
- Monitor for clones of PsExec such as RemCom (named pipes `\\.\pipe\remcom_comunication` and binary internal name `remcom`), PAExec (binary internal name containing `paexec`) or CSExec (named pipe `\\.\pipe\csexecsvc` and a binary internal name containing `csexec`).
- Monitor for unique PsExec naming conventions: `<psexecsvc_name><machine_name><5_random_digits><stdin|stdout|stderr>`.

## Associated Techniques

- [T1569.002](https://attack.mitre.org/techniques/T1569/002/) - System Services: Service Execution
- [T1036.004](https://attack.mitre.org/techniques/T1036/004/) - Masquerading: Masquerade Task or Service 

## Related Countermeasures

- CM0001 - Disable Server Message Block (SMB) Protocol
- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0017 - Enable Port Filtering on Host-based Firewalls
- CM0060 - Restrict and Monitor Domain Accounts with Local Administrator Access to Workstations
- CM0062 - Monitor or Block Server Message Block (SMB) Protocol 
- CM0036 - Identify and Monitor Remote Access Tools

## References

- PsExec v2.43 | <https://learn.microsoft.com/en-us/sysinternals/downloads/psexec>
- Threat Hunting for PsExec, Open-Source Clones, and Other Lateral Movement Tools | <https://redcanary.com/blog/threat-detection/threat-hunting-psexec-lateral-movement/>
- Attack surface reduction rules reference | <https://learn.microsoft.com/en-us/defender-endpoint/attack-surface-reduction-rules-reference>