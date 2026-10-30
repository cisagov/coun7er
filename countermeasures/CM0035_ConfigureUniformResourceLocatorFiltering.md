# Configure Uniform Resource Locator (URL) Filtering

* **ID:** CM0035
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Infrastructure Support (PD-WRL-004)
* **Skills & Knowledge:** Web gateway administration; URL filtering and proxy controls

## Intended Outcome

Configuring Uniform Resource Locator (URL) filtering restricts adversary initial access and execution via  malicious URLs.

## Introduction

Uniform Resource Locator (URL) filtering, also known as web filtering, is used to control access to web pages by permitting or denying access when a user clicks on a link. URL filtering blocks compromised webpages used by adversaries to facilitate phishing attacks and malicious code execution.   

URL filtering operates similarly to Domain Name System (DNS) filtering, although the latter blocks DNS query requests for a domain.

Note that adversaries are able to generate new URLs, so manually updating URL filtering will not be sufficient for full protection against adversaries phishing attempts and should be paired with other, more sophisticated, methods of web security. 

## Preparation

- No Preparation content identified.

## Risks

- Blocking legitimate URLs may disrupt business operations. 

## Guidance 

A URL filter is useful to configure for instances where a webpage on a domain is known to be or has been compromised or to meet business objectives. 

### Evaluating URLs during an Incident

* Check URLs against IoCs.
* Check clickable links/downloads available on the webpage.
* Check HTTP headers and Payloads using an intercept proxy such as, OWASP ZAP, to the site. 
* View network traffic to suspicious URK using a protocol analyzer such as WireShark
    

### Applying a Filter

Applying a filter can be based on an allow-list, block-list, or both. 
There are multiple commercial tools that may be used to block malicious URLs (Cisco Umbrella, ZScaler, Palo Alto,  etc). Organizations should consult their vendor for specific steps to block a known malicious URL. Commercial tools will also come with pre-built categories or URLs for preventative or post measures. Another, preventative step for consideration is blocking common mistypings of popular or common URLs used by staff to prevent typosquatting attacks. 

## Associated Techniques

- [T1566](https://attack.mitre.org/techniques/T1566/) - Phishing
- [T1566.002](https://attack.mitre.org/techniques/T1566/002/) - Phishing: Spearphishing Link
- [T1566.003](https://attack.mitre.org/techniques/T1566/003/) - Phishing: Spearphishing via Service 
- [T1189](https://attack.mitre.org/techniques/T1189/) - Drive-by Compromise
- [T1204](https://attack.mitre.org/techniques/T1204/) - User Execution 
- [T1204.001](https://attack.mitre.org/techniques/T1204/001/) - User Execution: Malicious Link 

## Related Countermeasures

- CM0009 - Update Domain Name Service (DNS) Deny List
- CM0010 - Enable Internet Protocol (IP) Address Allowlists
- CM0011 - Enable Domain Name System (DNS) Sinkhole
- CM0034 - Identify and Block Suspicious Hosts
- CM0040 - Clear Workstation and Server Domain Name Server (DNS) Caches

## References

- What is URL Filtering | <https://www.zscaler.com/zpedia/what-is-url-filtering>
- URL Filtering | <https://www.cisco.com/c/en/us/td/docs/routers/sdwan/configuration/security/ios-xe-16/security-book-xe/url-filtering.pdf>
- Threat Hunting URLs for URLs as an IoC | <https://www.cisco.com/c/en/us/td/docs/routers/sdwan/configuration/security/ios-xe-16/security-book-xe/url-filtering.pdf>
- How to prevent and protect from typosquatting | <https://www.redpoints.com/blog/prevent-typosquatting/>
