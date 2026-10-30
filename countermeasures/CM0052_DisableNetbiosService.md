# Disable NetBIOS Service

* **ID:** CM0052
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable 
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows networking; NetBIOS and legacy name services

## Intended Outcome 

Disabling the NetBIOS service blocks adversary initial access and lateral movement directives that exploit the NetBIOS service and port pairing.

## Introduction 

NetBIOS (Network Basic Input/Output System) is an older network protocol used to facilitate communication between applications on different computers within a local area network (LAN). NetBIOS played a pivotal role in early networking communications and error recovery, although it is now often an unnecessary addition to the session layer of a network. NetBIOS is able to be exploited by attackers to gather computer names, account credentials, and shared network resources which may be leveraged for initial access or lateral movement within a network. For these reasons, it is recommended to disable the NetBIOS service entirely; if this cannot be done, NetBIOS traffic should be restricted to specific IP addresses and should be blocked to/from the Internet.

## Preparation 

- Assess the network, identifying service/application dependencies on NetBIOS and impact to operations if NetBIOS is disabled or restricted.
- Conduct a service scan to detect all devices and interfaces where NetBIOS is enabled.
- Conduct risk assessment with relevant IT staff and stakeholders to determine whether potential disruption outweighs the consequence of increased exposure to adversary access and lateral movement.

## Risks 

- This countermeasure can disrupt functionality of systems that rely on NetBIOS.
- Disabling NetBIOS or restricting traffic under the service may result in compatibility issues with older networked applications and services.

## Guidance 

### Disabling Action

- By default, NetBIOS uses TCP port 139 to provide session services, and uses UDP ports 137 and 138 to provide name services and datagram services respectively. System administrators should disable NetBIOS over TCP/IP on all devices across the network.
- The NetBIOS service is disabled via network adapter settings and can be functionally restricted to certain IP addresses using the Windows Firewall utility.
- NetBIOs cannot be disabled via Group Policy adjustments, although it can be disabled programmatically through PowerShell. The following PowerShell script uses Windows Management Instrumentation (WMI) service to enumerate all IP enabled network interfaces to set all NIC's NetBIOS property to disabled:
`
$NETBIOS_DISABLED=2
$NETBIOS_DISABLED=0
Get-WmiObject Win32_NetworkAdapterConfiguration -filter "ipenabled = 'true'" | ForEach-Object { $_.SetTcpipNetbios($NETBIOS_DISABLED)}`
    

## Associated Techniques 

- [T1219](https://attack.mitre.org/techniques/T1219/) - Remote Access Software
- [T1046](https://attack.mitre.org/techniques/T1046/) - Network Service Discovery

## Related Countermeasures

- CM0001 - Disable Server Message Block (SMB) Protocol
- CM0019 - Disable or Restrict Windows Remote Management (WinRM) Protocol
- CM0020 - Disable Virtual Network Computing (VNC) Desktop-sharing System
- CM0051 - Disable Bluetooth
- CM0053 - Disable Link-Local Multicast Name Resolution (LLMNR) Service
- CM0098 - Enable Extended Protection for Authentication (EPA)

## References 

- Weak Security Controls and Practices Routinely Exploited for Initial Access | <https://www.cisa.gov/news-events/cybersecurity-advisories/aa22-137a>
- NetBIOS (Network Basic Input/Output System) | <https://www.techtarget.com/searchnetworking/definition/NetBIOS>
- NetBIOS and SMB Penetration Testing on Windows | <https://www.hackingarticles.in/netbios-and-smb-penetration-testing-on-windows/>
- How to disable NetBIOS over TCP/IP by using DHCP server options | <https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/disable-netbios-tcp-ip-using-dhcp>
- How to Disable NetBIOS and LLMNR | <https://blog.alexmags.com/posts/disable-netbios/>
