# Enable LDAP Signing and Channel Binding 

* **ID:** CM0142
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Systems Security Analysis (IO-WRL-006)
* **Skills & Knowledge:** Active Directory administration; LDAP signing and channel binding, Log Analysis, Policy audit

## Intended Outcome

Enabling Lightweight Directory Access Protocol (LDAP) signing, and channel binding prevents attempts at privilege escalation via LDAP relay attacks. 

## Introduction

LDAP is a protocol that is used by Active Directory (AD) for communication and authentication with directory services and applications. LDAP relay attacks abuse NTLM authentication by intercepting NTLM authentication requests and relaying the captured credentials to a domain controller’s LDAP interface. These adversary-in-the-middle attacks can result in privilege escalation.  

LDAP signing and channel binding provide a means of improving the security of communications between LDAP clients and Active Directory domain controllers. LDAP signing requires clients to sign LDAP requests with the client’s digital signature to ensure the integrity of the exchange. LDAP channel binding binds the TLS tunnel with the application layer, thereby preventing stolen LDAP tickets from being used to authenticate elsewhere.    

## Preparation

-	LDAP signing and channel binding is only supported on more recent versions of the Windows operating system (Windows 7 and higher). 
-	Identify systems that do not support LDAP signing and channel binding. Implement a comprehensive audit plan to identify non-compliant systems.
-   Update Windows systems and Group Policy administrative templates on Domain Controllers (DCs). 
-	Ensure robust logging and continual monitoring throughout implementation to identify issues, should they occur.

## Risks

Enabling LDAP signing and channel binding introduces the risk of operational interruption. To minimize this risk, it is critical that these countermeasures be scaled into operations and audited to ensure all systems can comply. Consult the “Guidance” section for the process of scaling the countermeasures to reduce the risk of operational interruption. 

## Guidance

### LDAP Audit Plan

Before enabling LDAP features of signing and channel binding, it is important to verify systems support said functionalities through an audit plan that verifies all necessary configurations are in place to enforce secure LDAP communication.
-	Group Policy Assessment:
    - Verify that Group Policies are configured to "Require Signing" for LDAP connections on all domain controllers.
    - Check if policies are set to enable LDAP channel binding where applicable.
    - Review the "Network security: LDAP client signing requirements" policy setting on domain controllers
-   Registry Settings:
	- Examine relevant registry keys to confirm the desired LDAP signing and channel binding configurations on specific servers.
-	Event ID Review:
	- Monitor for specific event IDs related to LDAP signing and channel binding on domain controllers, such as Event IDs 2886, 2887, 2888, 2889 (for LDAP signing) and 3039, 3040 (for channel binding).
-	Log Analysis:
	- Identify any instances of failed LDAP signing attempts or channel binding issues within the event logs
    - Analyze the source of these events to determine which clients or applications are making insecure LDAP connections.
-   Client Review:
	- Create an inventory of all devices that interact with the domain, such as workstations, servers, and applications.
    - Assess whether each client supports LDAP signing and channel binding based on their operating system and application versions.
    - Verify that client-side configurations are set to utilize LDAP signing and channel binding when possible.

### LDAP Signing

LDAP signing must be configured on both DCs and clients. 
-	Configure clients via GPO settings:
    - Create a group policy object
    - Navigate to `Computer Configuration>Policies>Windows Settings>Local Policies>Security Options`.
    - Change the “Network Security: LDAP Client Signing Requirement” setting from “None” to “Negotiate Signing.”
    - Apply the policy to all clients.

-	Configure DC via Group Policy
    - Elevate LDAP events diagnostic logging on all DCs:
        - `Reg Add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Diagnostics /v “16 LDAP Interface Events” /t REG_DWORD /d 2`.
    - Create a group policy object
    - Navigate to `Computer Configuration>Policies>Windows Settings>Local Policies>Security Options`.
    - Modify the “Domain Controller: LDAP Server Signing Requirements” from “None” to “Require Signing.”

-	Configure SIEM to alert on Directory Service Event IDs 2889 (signing requests that do not conform).
-	Audit and continually monitor for non-compliant clients. Ideally non-compliant clients should be remediated with the ultimate intent of requiring LDAP signing.
-	If all clients are compliant and there are no indications of operational interruption, edit the group policy object used to initially configure clients for LDAP signing and modify the “Network Security: LDAP Client Signing Requirements” from “Negotiate Signing” to “Require Signing.” 

### LDAP Channel Binding

-	Configure Group Policy settings on DCs:
    - Ensure LDAP event diagnostic logging is elevated on all DCs:
       - `Reg Add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Diagnostics /v “16 LDAP Interface Events” /t REG_DWORD /d 2`.
    - Create a group policy object.
    - Navigate to `Default Domain Controller Policy>Computer Configuration>Policies>Windows Settings>Security Settings>Local Policies` and select “Security Options.”
    - Right-click on “Domain Controller: LDAP Server Signing Requirements” and select “Properties.”
    - In the “Domain Controller: LDAP Server Signing Requirements Properties” box, enable “Define this Policy Setting.”
    - Select “When Supported” and confirm the setting change. 
    - Configure this group policy to log all bind failures on DCs. 
    - Configure SIEM to alert on Directory Service Event IDs 3039, 3074, 3075 (channel binding failure). 
    - Review logs to identify (source IP address) and then remediate non-compliant systems with the intent of ultimately requiring channel binding.
    - Once all systems are compliant, edit the group policy object used to initially configure LDAP channel binding by modifying the “LDAP Server Binding Token Requirements” setting to “Always.” 

## Associated Techniques

-	[T1557](https://attack.mitre.org/techniques/T1557/) – Adversary-in-the-Middle

## Related Countermeasures

- RM0101 - Enable SMB Signing to Stop NTLMv2 Relay
- CM0098 – Enable Extended Protection for Authentication (EPA)
- CM0111 - Enable Multiple Administrative Approval (MAA) for Access Policies
- CM0121 – Enable Managed Domain Authentication

## References

-	How to Enable LDAP Signing in Windows Server | <https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/enable-ldap-signing-in-windows-server>
-	What is LDAP Authentication | <https://fortinet.com/resources/cyberglossary/ldap-authentication>
-	LDAP Channel Binding and Signing | <https://hub.trimarcsecurity.com/post/ldap-channel-binding-and-signing>
