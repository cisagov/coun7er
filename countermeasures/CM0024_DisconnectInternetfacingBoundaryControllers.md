# Disconnect Internet-facing Boundary Controllers

* **ID:** CM0024
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Eliminate
* **Status:** Active
* **Nice Work Role:** Network Operations (IO-WRL-004)
* **Skills & Knowledge:** Network administration; boundary devices and internet connectivity

## Intended Outcome

Disconnecting Internet-facing boundary controllers terminates adversary
exfiltration and command and control via the Internet.

## Introduction

Disconnecting an enterprise from the Internet is a significant mitigation
step that involves physically or logically isolating the enterprise's
systems and data. Disconnection is sometimes necessary to limit the harm a potential/confirmed infection can cause to an organization. Extreme caution should be taken while implementing this countermeasure.

Performing an "all-at-once" disconnection of an enterprise network will stop communications between infected devices and the outside network. This has the potential added benefit of catching an adversary off-guard, thus limiting the response an adversary can prepare
or implement. While the enterprise network is disconnected from the
Internet, exfiltration of data/live control of any infected devices
within a network will be prevented. This will give incident responders
time to investigate and remediate any security incidents.

## Preparation

- Identify critical business & security systems.
- Identify systems that cannot be disconnected from the Internet. Identifying such systems ahead of time will lessen the countermeasure's business impact. 
- Develop a comprehensive plan that includes:
    - A complete list of business tasks that will be impacted, along with a list of temporary workarounds.
    - Alternative communication methods in case of communication impact.
    - Steps required to restore Internet connectivity.
    - List of personnel to coordinate with prior, during, and after disconnection takes place.
    - An estimated timeline to aid in investigation and planning.

## Risks

- This countermeasure can break legitimate functionality.
- Disconnecting an enterprise from the Internet will likely cause business interruption.
- Any Internet connections left active can reduce or eliminate the effectiveness of the countermeasure.

## Guidance

### Physical Disconnect

- Physically disconnecting any infected or suspected infected networks from the Internet is a straightforward method to carry out this mitigation. The exact steps for this will be dependent on the architecture of a network as well as the hardware deployed. 
- A thorough understanding of an organization’s network architecture is key to implementing this mitigation correctly.

### Logical Disconnect

- In some cases, a complete physical disconnection from the Internet is not feasible. For these situations, logical disconnection can be implemented at the firewall level by introducing firewall rules that will drop traffic to and from infected systems. This can be done by identifying infected endpoints and blocking/dropping packets designated to or from unique identifiable information (IP, MAC, etc.). 
- The proper documentation for the deployed firewall solution should be consulted for specific guidance on how to create these firewall rules.

## Associated Techniques

- [T1036](https://attack.mitre.org/techniques/T1036/) - Masquerading
- [T1587.001](https://attack.mitre.org/techniques/T1587/001) - Develop Capabilities: Malware

## Related Countermeasures

- RM0071 - Validate Internal Network Firewall Rulesets (*future*)
- CM0023 - Identify and Terminate Suspicious Processes
- RM0273 - Reduce Internet Access (*future*)
- RM0274 - Implement Host-based Firewalls (*future*)
- RM0276 - Validate Perimeter Firewall Rulesets (*future*)
- CM0065 - Isolate Endpoints from Network
- CM0106 - Eliminate Web Shells

## References

- Cybersecurity Incident & Vulnerability Response Playbooks | <https://www.cisa.gov/sites/default/files/2024-08/Federal_Government_Cybersecurity_Incident_and_Vulnerability_Response_Playbooks_508C.pdf>
- Eviction Guidance for Networks Affected by the SolarWinds and Active Directory/M365 Compromise | <https://www.cisa.gov/news-events/analysis-reports/ar21-134a>
- Remediating Networks Affected by the SolarWinds and Active Directory/M365 Compromise: Risk Decisions for Leaders | <https://www.cisa.gov/sites/default/files/publications/CISA_Insights_SolarWinds-and-AD-M365-Compromise-Risk-Decisions-for-Leaders_0.pdf>
