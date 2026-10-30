# Identify and Block Suspicious Hosts

* **ID:** CM0034
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Defensive Cybersecurity (PD-WRL-001)
* **Skills & Knowledge:** Network/security monitoring; host identification and blocking

## Intended Outcome

Identifying and blocking suspicious hosts restricts adversary initial
access and privilege escalation from malicious hosts.

## Introduction

No Introduction content identified. 

## Preparation

- Take inventory of all hosts and services on the network.
- Establish a baseline of normal activity to more accurately detect suspicious and irregular events.
- Commercially solutions are available to identify and block suspicious hostnames. Determine which solution(s) will satisfy environmental requirements.

## Risks

- This countermeasure may have impacts that disrupt operations.
- The anomaly detection or intrusion prevention system may identify legitimate hosts as suspicious.

## Guidance

Identifying suspicious hostnames can be achieved through a combination of these tools and techniques:

- Network Monitoring Tools
    - Leverage network monitoring tools to identify suspicious hostnames by way of anomalous network activity or unauthorized connections. 
- Security Information and Event Management (SIEM) System
    - Use a SIEM to collect, aggregate, and analyze all available log data to identify suspicious hostnames and abnormal activity.  
- Host Discovery and Port Scanning
    - Use a host discovery or port scanning tool to take inventory of hosts and open ports.  Use this information to determine if a host and/or port is legitimate.       

## Associated Techniques

- [T1078](https://attack.mitre.org/techniques/T1078) - Valid Accounts
- [T1199](https://attack.mitre.org/techniques/T1199) - Trusted Relationship

## Related Countermeasures

- CM0010 - Enable Internet Protocol (IP) Address Allowlists
- CM0023 - Identify and Terminate Suspicious Processes
- CM0058 - Detect and Block Execution of Vulnerable Drivers
- CM0064 - Investigate Account Manipulation
- CM0065 - Isolate Endpoints from Network
- CM0035 - Configure Uniform Resource Locator (URL) Filtering

## References

- Network Monitoring Tools | <https://www.manageengine.com/network-monitoring/network-monitoring-tools.html>
- Top 10 Network Behavior Anomaly Detection Tools in 2022 | <https://www.spiceworks.com/tech/networking/articles/network-behavior-anomaly-detection-tools/>
- What is SIEM? | <https://www.ibm.com/topics/siem>
- 6 Best Network Discovery Tools - What is Network Discovery | <https://www.dnsstuff.com/network-discovery-tools#what-is-network-discovery>
