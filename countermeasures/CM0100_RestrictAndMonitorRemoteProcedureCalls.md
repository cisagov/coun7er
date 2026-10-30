# Restrict and Monitor Remote Procedure Calls (RPC)

* **ID:** CM0100
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows/network administration; RPC services, firewall rules, and monitoring

## Intended Outcome

Restricting and monitoring Remote Procedure Calls (RPC) limits opportunities for an adversary to conduct reconnaissance and achieve execution, lateral movement, and privilege escalation via RPC abuse. 

## Introduction

Remote Procedure Call (RPC) is a protocol native to the Windows operating system that enables inter-process communication (IPC) both locally and remotely. RPC is ubiquitous in a Windows environment and although necessary, significantly expands the attack surface.  RPC can be abused by adversaries to achieve reconnaissance, execution, lateral movement, and privilege escalation.   

RPC cannot be completely blocked, but it can be filtered and monitored. RPC filtering is a capability native to Windows that is facilitated by the Windows Filtering Platform (WFP) via netsh.exe.  Another option to consider is the [RPC Firewall](https://github.com/zeronetworks/rpcfirewall) by Zero Networks. This free and open-source tool may offer additional capabilities to enable defenders to more granularly filter and monitor RPC traffic.   

## Preparation

-	Audit RPC traffic to assess a baseline and identify anomalous traffic.
-	RPC filtering can be used to proactively prevent RPC abuse. A deny list can be created to block RPC interfaces and reduce the attack surface introduced by the protocol.

## Risks

RPC filtering is a complex topic that requires responders to be familiar with both the protocol and it’s attack surface. Inadvertently blocking necessary RPC traffic can introduce the risk of operational disruption.    

## Guidance

### Restrict

RPC can be restricted by leveraging native RPC filtering capabilities via netsh.exe or by implementing the RPC Firewall. Consider filtering, at a minimum, the following:
    - Permit only specific privileged users to create services remotely.
    - Permit only specific hosts to modify registry data remotely.
    - Prevent scheduled tasks from being created remotely.
    - Only permit Kerberos authentication to a specific RPC endpoint.
    - Block connections to an RPC endpoint made over specific named pipes.

#### RPC Filtering via netsh.exe

-	Consider the following process to create an RPC filter:
    - Add a rule - `netsh rpc filter add rule…`
    - Add conditions to the rule - `add condition field…`
    - Add the rule to a filter - `add filter…`

#### RPC Filtering via RPC Firewall

-	Consider the following process to create an RPC filter:
    - Identify the Universally Unique Identifier (UUID) and the Opnum (the number of a specific function of an RPC interface) of the RPC call.
    - Collect whitelisted IPs of endpoints that should be permitted to access RPC methods.
    - Block/allow/audit the call.

### Monitor

-	Monitoring RPC traffic is necessary to prevent and detect RPC abuse. RPC auditing can be implemented via the WFP and netsh.exe or via the RPC Firewall. Successful and failed connections should be captured in the Security Event Log and forwarded to a centralized location for detection and analysis.    

## Associated Techniques 

- [T1021.003](https://attack.mitre.org/techniques/T1021/003/) – Remote Services: Distributed Component Object Model
- [T1210](https://attack.mitre.org/techniques/T1210/) – Exploitation of Remote Services


## Related Countermeasures

-	CM0001 – Disable Server Message Block (SMB) Protocol
-	CM0012 – Disable or Restrict Distributed Component Object Model (DCOM) Protocol
-	CM0062 – Monitor or Block Server Message Block (SMB) Protocol

## References

- Stopping Lateral Movement via the RPC Firewall | <https://zeronetworks.com/blog/stopping-lateral-movement-via-the-rpc-firewall>
- A Definitive Guide to the Remote Procedure Call (RPC) Filter | <https://www.akamai.com/blog/security/guide-rpc-filter>
- Filtering Layer Identifiers | <https://learn.microsoft.com/en-us/windows/win32/fwp/management-filtering-layer-identifiers->
- The Dark Side of Microsoft Remote Procedure Call Protocols | <https://redcanary.com/blog/threat-detection/msrpc-to-attack/>
- RPC Firewall | <https://github.com/zeronetworks/rpcfirewall>
