# Enable Extended Protection for Authentication (EPA) 

* **ID:** CM0098
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active

## Intended Outcome 

Enabling Extended Protection for Authentication on clients and servers restricts adversary credential access and lateral movement facilitated by man-in-the-middle (MITM) or relay attacks.

## Introduction 

Man-in-the-middle attacks such as NTLM Relay attacks are often observed in adversarial attempts at credential access that involve certificate authority (CA) impersonation and authentication request interception. The PetitPotam attack is a prime example of an NTLM Relay attack that targets Active Directory Certificate Services (AD CS) lacking proper protection such as client-authenticated TLS channels. Extended Protection for Authentication (EPA) is a feature that provides protection for authentication protocols such as NTLM and Kerberos by binding the session key generated by Secure Sockets Layer (SSL) when a TLS channel is established to the session key that is generated during a client's attempted authentication. Enabling EPA prevents malicious tradecraft observed in the PetitPotam attack where legitimate authentication requests are captured and replayed by an attacker to an AD CS server.

Similarly, adversary MITM attacks have been observed exploiting Microsoft Exchange Server systems where EPA is absent. For this reason it is important to properly configure Exchange Server with Extended Protection.

## Preparation 

- Identify all systems and services within the network that rely on NTLM or Kerberos for authentication. Prioritize identification of systems that utilize AD CS.
- Ensure all identified systems are upgraded to the latest security patches and update.
- Inform administrative personnel about the intent to enable EPA on the client and server side of authentication infrastructure.
- Identify the Exchange Server version that is present. Take note of Exchange Servers that are published using Hybrid Agent.
-  When enabling EPA for Exchange Server, SSL flags of `SSL` and `SSL128` are required to be present. 
- SSL offloading for Outlook Anywhere must be disabled before EPA is enabled in Exchange Server: [Exchange Server support for Windows Extended Protection | Microsoft Learn](https://learn.microsoft.com/en-us/exchange/plan-and-deploy/post-installation-tasks/security-best-practices/exchange-extended-protection?view=exchserver-2019) 

## Risks 

- Enabling EPA may incite compatibility issues with older applications or services that do not support EPA such as RPC.
- Enabling EPA is a delicate process that incurs administrative overhead on client and server AD CS machines.
- Misconfiguration of Windows Authentication features increase the vulnerability of AD CS machines to authentication channel abuse.
- Enabling Extended Protection on Exchange Servers that are published via Hybrid Agent may incite disruption of features such as mailbox moves and free/busy calls.

## Guidance 

### Enable EPA for Certificate Authority Web Enrollment (CertSrv)

- Access the Internet Information Services (IIS) Manager in order to enable EPA for each web service or application listed under the `Sites` pane of the respective IIS Server.
- Ensure that **Windows Authentication** is set to **Enabled** in the **Authentication** pane.
- Enter the **Advanced Settings** of the **Windows Authentication** feature to set **Extended Protection** to **Required**

### Enable EPA for Certificate Enrollment Web Service (CES) Role

- Repeat the aforementioned steps for the Certificate Enrollment Web Service role.
- Ensure updates to the *Web.config* file generated by the CES Role by adding `<extendedProtectionPolicy>` set with a value of **Always**.

### Enable EPA for Microsoft Exchange Server

- Windows extended protection is also relevant when enhancing existing authentication measures in Microsoft Exchange Server. It is enabled by default in Exchange Server 2019 (and newer) versions.
- Implement EPA for older Exchange Server versions by downloading the `ExchangeExtendedProtectionManagement.ps1` script from Microsoft: [ExchangeExtendedProtectionManagement - Microsoft - CSS-Exchange](https://microsoft.github.io/CSS-Exchange/Security/ExchangeExtendedProtectionManagement/)
- Follow the supplied directives on proper execution syntax of the `ExchangeExtendedProtectionManagement.ps1` script based on discretion to exclude specific servers from the enablement of EPA.

## Associated Techniques 

- [T1557.001](https://attack.mitre.org/techniques/T1557/001/) - Adversary-in-the-Middle: LLMNR/NBT-NS Poisoning and SMB Relay

## Related Countermeasures

- CM0001 - Disable Server Message Block (SMB) Protocol
- CM0019 - Disable or Restrict Windows Remote Management (WinRM) Protocol
- CM0020 - Disable Virtual Network Computing (VNC) Desktop-sharing System
- CM0052 - Disable NetBIOS Service
- CM0053 - Disable Link-Local Multicast Name Resolution (LLMNR) Service
- CM0029 - Reset NT Hashes for Smart Card-enabled Accounts
- CM0069 - Reset Directory Services Restore Mode (DSRM) Account Passwords on Domain Controllers
- CM0142 - Enable LDAP Signing and Channel Binding

## References 

- Extended Protection for Authentication Overview | <https://learn.microsoft.com/en-us/dotnet/framework/wcf/feature-details/extended-protection-for-authentication-overview>
- KB5005413: Mitigating NTLM Relay Attacks on Active Directory Certificate Services (AD CS) | <https://support.microsoft.com/en-us/topic/kb5005413-mitigating-ntlm-relay-attacks-on-active-directory-certificate-services-ad-cs-3612b773-4043-4aa9-b23d-b87910cd3429>
- PetitPotam attack on Active Directory Certificate Services: How to mitigate NTLM Relay PetitPotam attacks on AD CS | <https://community.veeam.com/blogs-and-podcasts-57/petitpotam-attack-on-active-directory-certificate-services-how-to-mitigate-ntlm-relay-petitpotam-attacks-on-ad-cs-1202>
- Configure Windows Extended Protection in Exchange Server | <https://learn.microsoft.com/en-us/exchange/plan-and-deploy/post-installation-tasks/security-best-practices/exchange-extended-protection?view=exchserver-2019>
- Exchange Extended Protection Management | <https://microsoft.github.io/CSS-Exchange/Security/ExchangeExtendedProtectionManagement/>
- Microsoft Exchange Vulnerability Actively Exploited | <https://www.threatdown.com/blog/microsoft-exchange-vulnerability-actively-exploited/>
