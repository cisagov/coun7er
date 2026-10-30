# Enable Domain Name System (DNS) Sinkhole

* **ID:** CM0011
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Network Operations (IO-WRL-004)
* **Skills & Knowledge:** Network administration; DNS sinkholing and DNS infrastructure

## Intended Outcome

Enabling a Domain Name System (DNS) sinkhole blocks adversary command
and control (C2) using known C2 servers.

## Introduction

DNS sinkholes redirect clients attempting to connect to a malicious
domain to a benign destination. This capability can be paired with
servers/listeners to emulate normal transactions. The integration of an
intrusion detection system (IDS) enables real-time alerting and regular
reporting.

## Preparation

- DNS BIND
    - Deploy BIND to publish and resolve DNS information over the internet.
    - Modify the zone configuration file (/etc/named.conf). This file specifies options for BIND, what to log, and which zones to load. Specify organization-specific settings and sink hole zones for blocked domains.
- Publicly Available Lists
    - Confirm that lists do not include extranets or business partners.
    - Note the Time to Live (TTL) assigned to all domains. The TTL should be modified according to the requirement of immediacy. These changes will need to propagate to all other DNS servers in the organization's configuration to take effect.
    - Remove duplicate domains and subdomains of any top-level domains from the list.
- Custom Lists
    - Each time a host is observed connecting to a domain associated with adversarial activity, an entry should be made to prevent other hosts from interacting with said domain.
- Restart DNS Service
    - Point /etc/resolve.conf to DNS server IP.
    - Test using dig and nslookup to ensure domains are either blocked or resolving properly.
    - Capture log to find infected hosts.

## Risks

- This countermeasure may not stop all ongoing activity.
    - Malware may evade sinkholing by hard-coding IP addresses and using them for C2.
    - A newly-sinkholed domain may continue to resolve until the domain record's Time to Live (TTL) expires.
- This countermeasure may block legitimate activity.
    - Valid domains may be accidentally sinkholed.

## Guidance

Enable the monitoring and logging of systems that hit the DNS sinkhole server. Sinkhole server logs can assist in identifying compromised hosts in scenarios in which the firewall does not provide visibility into the origins of DNS queries.

- Configure a dedicated sinkhole server to ensure maximum logging
- Consider running a packet capture tool persistently for full packet payloads using circular logging
- Configure SOcket CAT (Socat) to listen on specific ports. Socat can be configured to simulate a 3-way handshake to capture GET/POST data
    - Multiple listeners can be configured on the same IP address
    - Alternatively, if you elect to establish multiple sinkholes, Socat can be configured to listen on multiple IP addresses
- Configure an Intrusion Detection System (IDS) such as Snort, to enable the sinkhole to capture and alert on redirects to the sinkhole in real-time
- Consider configuring regular (daily) reporting on sinkhole traffic

## Associated Techniques

No Associated Techniques content identified.

## Related Countermeasures

- CM0009 - Update Domain Name Service (DNS) Deny List
- CM0035 - Configure Uniform Resource Locator (URL) Filtering
- CM0040 - Clear Workstation and Server Domain Name Server (DNS) Caches
- CM0093 - Prevent and Detect Network Traffic to/from The Onion Router (Tor) Exit Nodes

## References

- How DNS Sinkholing Works | <https://docs.paloaltonetworks.com/pan-os/9-1/pan-os-admin/threat-prevention/use-dns-queries-to-identify-infected-hosts-on-the-network/dns-sinkholing>
- Understanding DNS sinkholes - A weapon against malware [updated 2021] | <https://www.infosecinstitute.com/resources/general-security/dns-sinkhole/>
- DNS sinkhole: A tool to help thwart cyberattacks | <https://bluecatnetworks.com/blog/dns-sinkhole-a-tool-to-help-thwart-cyberattacks/>
- Network Admin's Guide to Synthetic Monitoring | <https://www.catchpoint.com/network-admin-guide/dns-sinkhole>
- How to Configure DNS Sinkholing in the Firewall | <https://campus.barracuda.com/product/cloudgenfirewall/doc/98210196/how-to-configure-dns-sinkholing-in-the-firewall/>
- Build Securely a DNS Sinkhole Step-by-Step Powered by Slackware Linux | <https://handlers.sans.org/gbruneau/docs/DNS_Sinkhole_setup.pdf>
- DNSBlackHole | <https://github.com/yoda66/DNSBlackHole>
- SEC505 Scripts | <https://blueteampowershell.com/SEC505-Scripts.zip>
