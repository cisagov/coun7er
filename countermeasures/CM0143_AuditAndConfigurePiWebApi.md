# Audit and Configure PI Web API

* **ID:** CM0143
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** PI System administration; PI Web API authentication and access configuration, API intergration

## Intended Outcome

Auditing and configuring the Plant Information (PI) Web Application Programming Interface (API) prevents an adversary's attempts to perform discovery and achieve remote code execution via the API.  

## Introduction

The PI Web API is a REpresentational State Transfer (RESTful) API allowing client applications access to PI and Asset Framework (AF) data over the secure HyperText Transfer Protocol (HTTPS). The API is used in critical manufacturing sectors to monitor and manage infrastructure and industrial systems. 

Adversaries may use malicious XML imports to execute remote code in the API environment with the privileges of an interactive user. Additionally, if an adversary gains access to the PI Web API, they may access the plant information and change settings to cause damage to the infrastructure the API manages. 

## Preparation

- Identify organization's desired configuration settings. 
- Ensure user account performing configuration changes has appropriate permissions. May need to be a PI Web API administrator. 

## Risks

- Consider configuring settings outside of active business hours to reduce the risk of operational disruption. 

## Guidance 

The PI Web API can be configured both during installation and during runtime. Ensure that the currently configured settings are the desired ones for the organization. 

### Configure installation settings

- To start configuration, select `Yes, I would like to participate` in the PI System Customer Experience Improvement Program pane. 
- Select server on which to store configuration data. By default, this is the last-configured server. To change the server, navigate to `... > PI AF Servers` and select the desired server. 
- Select the authentication methods for the API. Bearer and Kerberos are mutually exclusive and Anonymous authentication disables all other types. Other methods may be combined. 
	- OpenID Connect/Bearer
	- Kerberos
	- Basic
	- None/Anonymous (Aveva recommends against allowing the Anonymous authentication type.)
- Select the communication port for PI Web API. The default is port 443 for HTTPS traffic; it is largely recommended to use default ports. 
- If remote clients need to access the service, configure a firewall exception by keeping the `Yes, please create a Firewall Exception for PI Web API` checked. Otherwise uncheck the box. 
- Select an SSL certificate for the communication port. 
- Select the account under which the service will operate. 
- Select where the service will store Open Message Format (OMF) data; the default value is the currently-connected PI AF server.
- Review the settings, apply if correct. 

### Configuration with Admin Utility

The Admin Utility allows users to change any of the settings configured during installation. It is located under `Start menu > PI System`. The following settings should only be edited via Admin Utility, as other methods may result in errors or cause the service to stop working:

- Telemetry
- Configuration Store
- Authentication Option
- AIM Registration
- Listen Port
- Certificate
- API Service
- OMF Servers
Aveva recommends not using anonymous authentication and keeping communication ports as the default 443, but other configuration settings are up to the organization. 

### Configuration at runtime

- Within the RESTful interface, configuration settings are located under `/piwebapi/system/configuration`. Settings can be edited, added, or removed there. 
- In the PI System Explorer, configuration values can be modified under `Elements > OSIsoft > PI Web API > Configuration Instance Name`, where the configuration instance name is the hostname of the server on which PI Web API is installed. 

## Associated Techniques

- [T0871](https://attack.mitre.org/techniques/T0871/) - Execution through API
- [T0846](https://attack.mitre.org/techniques/T0846/) - Remote System Discovery
- [T0888](https://attack.mitre.org/techniques/T0888/) - Remote System Information Discovery

## Related Countermeasures

- CM0047 - Block High-risk File Types at Web Gateway
- CM0017 - Enable Port Filtering on Host-based Firewalls
- CM0083 - Rotate Application Programming Interface (API) Keys

## References

- PI Web API Reference | <https://docs.aveva.com/bundle/pi-web-api-reference/page/help.html>
- AVEVA PI Web API | <https://www.cisa.gov/news-events/ics-advisories/icsa-24-163-02>
- Configure installation settings | <https://docs.aveva.com/bundle/pi-web-api/page/1023088.html>
- Configuration with PI Web API Admin Utility | <https://docs.aveva.com/bundle/pi-web-api/page/1023087.html>
- Configuration at runtime | <https://docs.aveva.com/bundle/pi-web-api/page/1023022.html>
