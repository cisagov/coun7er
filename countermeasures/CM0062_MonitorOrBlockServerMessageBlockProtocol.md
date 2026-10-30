# Monitor or Block Server Message Block (SMB) Protocol

* **ID:** CM0062
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine, Disable
* **Status:** Active
 * **Nice Work Role:** Network Operations (IO-WRL-004)
* **Skills & Knowledge:** Network monitoring; SMB traffic and protocol filtering

## Intended Outcome

Monitoring and/or blocking the Server Message Block (SMB) protocol detects and/or blocks adversary lateral movement and command and control using SMB.  

## Introduction

No Introduction content identified.

## Preparation

-	Determine the extent to which SMB is used in the environment.
-	Determine the extent to which SMB has been secured in the environment.
-	Identify distinctions between legitimate and malicious SMB usage.
-	Identify opportunities for data collection.

## Risks

-	Blocking SMB can impact the operation or functionality of systems in a domain by obstructing access to shared files, data, and/or devices.
-	To minimize the likelihood of operational disruption, continually assess the effectiveness and efficiency of monitoring, collection, and detection.  Take steps to minimize the likelihood of false positives.

## Guidance

### Monitor

#### Collection

-	Host-based agents should, to the extent possible, be configured to monitor files, processes, named pipes, logon sessions, network shares, network traffic, and command-line execution.  These measures could provide insight into SMB abuse.    
-	Collect host-based logs (EDR, event logs, Sysmon, etc.).  If available, consider collecting logs for process creation, file creation, named pipe creation, named pipe connection, and network connections.  Consider reviewing windows security events for process creation, remote network authentication, special logons, and network share logs.  Investigate instances of process execution from admin shares and file write on admin shares. 

#### Detection

-	Consider implementing detection logic that monitors for the following 3 conditions within a 10-minute period:
    -   Successful logon event
    -   A process launches in the ADMIN$ path on a remote host
    -   A file share being opened repeatedly

### Block

-	Restrict or block all SMB communication between workstations to limit the likelihood of abusing the protocol to move laterally.  
-	Restrict or block SMB communications from workstations to servers where the business need for the protocol does not exist.


## Associated Techniques

-	[T1021.002](https://attack.mitre.org/techniques/T1021/002/) - SMB/Windows Admin Shares
-   [T1572](https://attack.mitre.org/techniques/T1572/) - Protocol Tunneling

## Related Countermeasures

- CM0001 - Disable Server Message Block (SMB) Protocol
- CM0020 - Disable Virtual Network Computing (VNC) Desktop-sharing System
- CM0100 - Restrict and Monitor Remote Procedure Calls (RPC)
- CM0141 - Audit and Restrict PsExec

## References

- Restricting SMB Based Lateral Movement in a Windows Environment | <https://blog.palantir.com/restricting-smb-based-lateral-movement-in-a-windows-environment-ed033b888721>
- Offensive Lateral Movement | <https://posts.specterops.io/offensive-lateral-movement-1744ae62b14f>
- Detecting Malicious C2 Activity - SpawnAs & SMB Lateral Movement in CobaltStrike | <https://dansec.medium.com/detecting-malicious-c2-activity-spawnas-smb-lateral-movement-in-cobaltstrike-9d518e68b64>
- SMB/Windows Admin Shares | <https://redcanary.com/threat-detection-report/techniques/windows-admin-shares/>
