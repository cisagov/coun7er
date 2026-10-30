# Clear Workstation and Server Domain Name Server (DNS) Caches

* **ID:** CM0040
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Eliminate
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; DNS cache management and name resolution

## Intended Outcome

Clearing workstation and server Domain Name Server (DNS) cache terminates collection and command-and-control (C2) via corrupted DNS lookups.

## Introduction

The domain name system (DNS) cache is used to expedite the resolution of DNS queries for recently visited domains. Adversaries can poison the DNS cache by inserting or replacing records in the cache resulting in victims communicating with adversary-controlled infrastructure. By clearing the DNS cache, all stored queries, including malicious or corrupted IP-Domain mappings, will be purged. 

## Preparation

- Capture all relevant forensic information before flushing cache.
- Ensure appropriate privileges are possessed by the user account issuing commands to clear the DNS cache.

## Risks

- Clearing the DNS cache will remove important forensic artifacts and IOCs. Ensure forensic captures of the DNS cache are taken prior to flushing. 

## Guidance

### Clear DNS Cache

**Windows**
* To flush DNS on a local Windows device with Command Prompt, the command: `ipconfig /flushdns` can be used. 
* To clear DNS on a Windows DNS server, the command-line tool Dnscmd can be used  with `dnscmd <DNSServerName> /clearcache` 
* System administrators can use the PowerShell cmdlets `Clear-DnsServerCache` and `Clear-DnsClientCache` respectively to clear resource records. 

**Linux**

To flush DNS on a local Linux device with Terminal, the commands vary based on the DNS caching service installed.
* For `nscd`, use: `sudo /etc/init.d/nscd restart`
* For `dnsmasq`, use: `sudo /etc/init.d/dnsmasq restart`
* For `systemd-resolved`, use: `sudo systemd-resolve --flush-caches`. Confirm cache is cleared with the command `sudo systemd-resolve --statistics` and confirm "Current Cache Size 0".

**MacOS**

Clearing the DNS cache on Mac workstations depends on the version of MacOS employed. For MacOS 10.10.4 and above the command is `sudo killall -HUP mDNSResponder`.

Clearing DNS records can be automated using scripts or using tools such as SolarWinds Server & Application Monitor. 

### Restart DNS Service

System administrators should restart the DNS services ensure the cache is cleared. For example, with nscd or dnsmasq, the service can be restarted with `sudo systemctl restart nscd` or `sudo systemctl restart dnsmasq` respectively. If Windows PowerShell is being used for a Windows DNS server, system administrators can use `Restart-Service -Name DNS -Force`.

## Associated Techniques

- [T1071.004](https://attack.mitre.org/techniques/T1071/004/) - Application Layer Protocol: DNS 

## Related Countermeasures

- CM0009 - Update Domain Name Service (DNS) Deny List
- CM0011 - Enable Domain Name System (DNS) Sinkhole
- CM0035 - Configure Uniform Resource Locator (URL) Filtering

## References

- Flush DNS: What It Is & How to Easily Clear DNS Cache | <https://blog.hubspot.com/website/flush-dns>
- Simplified Guide on How to Clear DNS Server Cache on Windows | <https://www.dnsstuff.com/clear-flush-dns-server-cache-windows>
- Dnscmd | <https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/dnscmd>
-  Eviction Guidance for Networks Affected by the SolarWinds and Active Directory/M365 Compromise | <https://www.cisa.gov/news-events/analysis-reports/ar21-134a>
