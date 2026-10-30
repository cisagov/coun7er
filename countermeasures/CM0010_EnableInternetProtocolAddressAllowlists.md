# Enable Internet Protocol (IP) Address Allowlists

* **ID:** CM0010
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Network Operations (IO-WRL-004)
* **Skills & Knowledge:** Network administration; IP allowlists and firewall rules


## Intended Outcome

Enabling Internet Protocol (IP) address allowlists restricts adversary
initial access and command and control using unverified IP addresses.

## Introduction

There are numerous open-source and commercially available solutions for
IP allowlisting.

## Preparation

No Preparation content identified.

## Risks

- This countermeasure may block legitimate activity.
- IP allowlisting risks blocking legitimate and/or mission essential connections.

## Guidance

On the host

- Check the firewall (Windows, UFW, etc.) for the presence of specific IP addresses for whitelisting.
- If not present, create an inbound rule for specifically approved IP addresses. 

On the Enterprise

- Maintain a list of allowed IP addresses.
- Import the allowlist into the endpoint detection/protection solution or via the main firewall.
- Process requests for adding new IP addresses to the allowlist.

## Associated Techniques

-   [T1071.004](https://attack.mitre.org/techniques/T1071/004) - Application Layer Protocol: DNS
-   [T1572](https://attack.mitre.org/techniques/T1572) - Protocol Tunneling
-   [T1557](https://attack.mitre.org/techniques/T1557) - Adversary-in-the-Middle

## Related Countermeasures

- CM0034 - Identify and Block Suspicious Hosts
- CM0035 - Configure Uniform Resource Locator (URL) Filtering
- CM0037 - Enable Geolocation-based Traffic Filtering
- CM0093 - Prevent and Detect Network Traffic to/from The Onion Router (Tor) Exit Nodes

## References

- Security - Firewall | <https://ubuntu.com/server/docs/firewalls>
- iptables(8) - Linux man page | <https://linux.die.net/man/8/iptables>
- How to Add IP Address in Windows Firewall | <https://web.archive.org/web/20240616112618/https://www.oryon.net/knowledge-base/article/how-to-add-ip-address-in-windows-firewall/>
- Uncomplicated Firewall | <https://wiki.archlinux.org/title/Uncomplicated_Firewall>
