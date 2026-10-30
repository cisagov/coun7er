# Rebuild Citrix NetScaler Appliances 

* **ID:** CM0081
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Network Operations (IO-WRL-004)
* **Skills & Knowledge:** Citrix NetScaler administration; appliance rebuilding and secure baselines

## Intended Outcome 

Rebuilding NetScaler appliances disrupts adversary discovery and credential access via session hijacking NetScaler Application Delivery Controller (ADC). 

## Introduction 

The Citrix NetScaler ADC is a network appliance typically architected between company firewall(s) and an application server located in a demilitarized zone (DMZ) that aims to increase efficiency of business operations through load balancing, network performance acceleration, and software/application distribution.

NetScaler ADCs are secured by NetScaler Gateways that provide administrators application-level policy controls and action controls, which secure access to applications and data accessible remotely through Secure Sockets Layer (SSL) (virtual private network) VPN technology. However, if an adversary is able to bypass credential provision or legitimate session creation by hijacking a session, they will be able to exfiltrate credentials, authentication tokens, and cookies. Given the prevalence of NetScaler ADCs in enterprise environments, and that the security of NetScaler ADCs is largely accounted for by proper configuration through NetScaler Gateway, proper remediating steps are required after compromise of user sessions or authentication tokens via NetScaler appliances.

## Preparation 

- Ensure isolation of NetScaler ADC and Gateway applications prior to application of patches or firmware upgrades.
- Correlate user session creation with anomalous data enumeration or transfer events.
- Identify whether the adversary compromised MFA tokens along with hijacked user sessions.

## Risks 

- Implementing this countermeasure will temporarily disrupt business operations and application access that is provided by NetScaler ADC.
- Upgrading firmware related to NetScaler ADC and Gateway will incur operational downtime, and in some cases may introduce compatibility issues and bugs to the appliances.
- Isolation of NetScaler appliances is very likely to sever load balancing and the timely access of business software.

## Guidance 

### Apply Upgrades/Updates to NetScaler Appliances

- Firsthand effort should be focused on updating and patching the NetScaler ADC and Gateway applications.
- In cases where the affected appliances cannot be prioritized for patching, ingress IP address restrictions need be implemented to constrain exposure and increased attack surface.
- Upgrade vulnerable NetScaler ADC and NetScaler Gateway appliances to their latest firmware versions after verifying upgrade integrity from vendor.

### Terminate Active Sessions

- Review the persistent and active sessions between NetScaler Gateway and NetScaler ADC separately. 
- Connect to the NetScaler appliances via CLI and kill all active sessions by specifying the relevant virtual server to target.
- Delegate API keys on an as-needed basis - keep secret and regularly audit API key usage to verify their usage by legitimate accounts.

### Credential Rotation 

- In the absence of log records or artifacts of exploitation activity, organizations should rotate the credentials of identities that were provisioned for accessing resources via NetScaler ADC or NetScaler Gateway appliances.
- Larger scope credential rotation is required in incidents where session hijacking is evident, and single factor authentication (SFA) remote access is permitted for resource access through the Internet.

### Rebuild NetScaler ADC/Gateway

- The presence of web shells or backdoors on NetScaler appliances begets network administrators to rebuild each appliance with a clean-source image and updated vendor firmware.
- Backup configurations should be reviewed to help ensure a lack of backdoors or loosely configured remote access tools.

## Associated Techniques 

- [T1539](https://attack.mitre.org/techniques/T1539/) - Steal Web Session Cookie
- [T1082](https://attack.mitre.org/techniques/T1082/) - System Information Discovery

## Related Countermeasures 

- CM0066 - Update Firmware
- CM0027 - Rebuild Compromised Host

## References 

- Citrix NetScaler ADC/Gateway: CVE-2023-4966 Remediation | <https://services.google.com/fh/files/misc/citrix-netscaler-adc-gateway-cve-2023-4966-remediation.pdf>
- #StopRansomware: LockBit 3.0 Ransomware Affiliates Exploit CVE 2023-4966 Citrix Bleed Vulnerability | <https://www.cisa.gov/sites/default/files/2023-11/aa23-325a_lockbit_3.0_ransomware_affiliates_exploit_cve-2023-4966_citrix_bleed_vulnerability_0.pdf>
- What is Citrix NetScaler, and how does it work? | <https://www.logicmonitor.com/blog/what-is-citrix-netscaler-and-how-does-it-work>
