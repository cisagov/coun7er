# Enable Geolocation-based Traffic Filtering

* **ID:** CM0037
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Network Operations (IO-WRL-004)
* **Skills & Knowledge:** Network administration; geolocation filtering and firewall policy

## Intended Outcome

Enabling geolocation-based traffic filtering restricts adversary
reconnaissance and initial access from geographic regions of concern.

## Introduction

Remote access logins and interactions from foreign nations into organizations network can be restricted using a number of methods depending on the specific requirements and available resources. Next-generation firewalls, content delivery networks (CDN), proxy servers, DNS services, and web application firewalls (WAFs) can all be configured to block traffic from specific regions based on geolocation. User identity management services can also be configured to block logins from specific geographic regions.

By blocking incoming requests from specific countries, a security operation center (SOC) may be able to optimize their network defenses by denying countries that perpetrate large amounts of low-level, automated
network probing.

## Preparation

- Confirm with business leadership that implementing the desired geo-block will not prevent critical business activities from taking place.


## Risks

- This countermeasure may have impacts that disrupt operations. For example, implementing geolocation traffic filtering may result in false-positives and blocking legitimate traffic, particularly if users apply VPN solutions before connecting into the network. 
- Geo-blocking will impair any legitimate interactions with entities in the blocked region.
- There may be legal restrictions on geo-blocking in certain regions, particularly within the European Union.

## Guidance

Blocking Outgoing Requests to Specific Countries

- If suspicious outbound traffic to a country of concern is discovered, investigate, and activate response measures according to the incident response plan.
- Consider implementing deny-all rules restricting outbound traffic to only approved countries or block outbound connections to countries deemed particularly high-risk.
- Consider implementing deception practices for externally facing systems accessible from specific regions of concern to enhance situational awareness and inform security operations.
- Monitor and investigate outgoing traffic from DMZ servers to suspicious geographic locations, particularly if a web application or network service is unlikely to have users located abroad.
- Consider implementing blocking and alerts on "impossible to travel" traffic - whereby a user logs in from multiple IP addresses that are geographically distant within the same time frame.

A variety of 3rd-party services, including user identity management, CDNs, and next-generation firewalls, provide support for geo-blocking. For instructions on establishing conditional access policies and geo-blocking, SOCs and network operation centers (NOCs) should consult vendor documentation used by their organization.  

## Associated Techniques

- [T1595](https://attack.mitre.org/techniques/T1595/) - Active Scanning
- [T1190](https://attack.mitre.org/techniques/T1190) - Exploit Public-Facing Application
- [T1592](https://attack.mitre.org/techniques/T1592) - Gather Victim Host Information
- [T1594](https://attack.mitre.org/techniques/T1594) - Search Victim-Owned Websites

## Related Countermeasures

- CM0002 - Enable Email Attachment Filtering and Message Authentication
- CM0010 - Enable Internet Protocol (IP) Address Allowlists
- CM0114 - Enable Microsoft Conditional Access Policy Templates

## References

- Geoblocking Best Practices: When Geoblocking Is (and Isn’t) Useful | <https://www.itprotoday.com/cloud-security/geoblocking-best-practices-when-geoblocking-is-and-isn-t-useful>
- NSA and CISA Recommend Immediate Actions to Reduce Exposure Across Operational Technologies and Control Systems | <https://www.cisa.gov/news-events/cybersecurity-advisories/aa20-205a>
- Q&A - Regulation on addressing unjustified geo-blocking | <https://www.ecommerce-europe.eu/wp-content/uploads/2019/03/Geo-blocking-QA.pdf>
