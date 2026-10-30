# Disable Link-Local Multicast Name Resolution (LLMNR) Service

* **ID:** CM0053
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable 
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows networking; LLMNR and name-resolution hardening

## Intended Outcome 

Disabling the Link-Local Multicast Name Resolution (LLMNR) service disrupts adversary credential access and collection via man-in-the-middle and LLMNR poisoning behaviors.

## Introduction 

Link-Local Multicast Name Resolution (LLMNR) is a protocol used by Windows to enable name resolution in scenarios where DNS is not present or fails to properly determine hostname and IP address associations. LLMNR operates by sending multicast queries over local networks that prompt an answer (IP address response) from all computers as to whether a certain hostname exists. LLMNR is exploited by attackers to perform man-in-the-middle (MitM) attacks and credential harvesting via LLMNR poisoning. For these reasons, it is recommended to disable the LLMNR service entirely- if this cannot be done, network segmentation should be established between LLMNR and DNS-based networks.

## Preparation 

- Assess the network, evaluating the current usage of LLMNR to understand why it is being used.
- Determine the systems and applications that rely on LLMNR for functionality.
- Conduct risk assessment with relevant IT staff and stakeholders to determine whether LLMNR can be effectively replaced by DNS implementation.
- Ensure availability of system backups in scenarios where registry changes are implemented to disable LLMNR.

## Risks 

- This countermeasure can disrupt business operations by causing network resolution failure.
- Disabling LLMNR or switching to DNS may lead to compatibility issues with specialized software.
- Disabling LLMNR encourages adoption of a DNS solution that requires alternative protections against attacks such as DNS Tunneling and denial-of-service.

## Guidance 

### Disable LLMNR on Windows Devices via GPO

- To prevent Windows machines from responding to or initiating LLMNR requests, navigate to the Local Group Policy Editor to set the "Turn off multicast name resolution" policy to "Enabled."
- The same group policy option can be applied to Active Directory Environments:
	- Computer Configuration -> Administrative Templates -> Network -> DNSClientEnable -> Turn Off Multicast Name Resolution -> Change value to "Enabled"
- *Note: the option may read as "Turn Off smart multi-homed name resolution" instead of "Turn Off Multicast Name Resolution." Change value to "Enabled" in both cases.*

### Disable LLMNR on Windows Devices via Registry

- Run the following commands in an elevated command prompt:
`REG ADD  “HKLM\Software\policies\Microsoft\Windows NT\DNSClient”`
`REG ADD  “HKLM\Software\policies\Microsoft\Windows NT\DNSClient” /v ”EnableMulticast” /t REG_DWORD /d “0” /f`
- This can also be accomplished by setting the value of the key `(HKLM\Software\Policies\Microsoft\Windows NT\DNSClient\EnableMulticast)` to zero.
- Document the aforementioned registry key changes.

### Disable LLMNR on Linux 

- Adjust the line "LLMNR=yes" to "LLMNR=no" within the `/etc/systemd/resolved.conf` file on Linux systems.
- Reboot the Linux system to properly disable LLMNR functionality.

### Configure Network Devices 

- Network devices such as routers and switches should be configured to block LLMNR traffic.
- Filter out LLMNR traffic on the multicast IPv4 address of `224.0.0.252`.
- After following configuration and disabling procedures, verify that LLMNOR is disabled by monitoring network traffic for LLMNR packets.

## Associated Techniques 

- [T1557.001](https://attack.mitre.org/techniques/T1557/001/) - Adversary-in-the-Middle: LLMNR/NBT-NS Poisoning and SMB Relay

## Related Countermeasures

- CM0001 - Disable Server Message Block (SMB) Protocol
- CM0051 - Disable Bluetooth
- CM0052 - Disable NetBIOS Service
- CM0098 - Enable Extended Protection for Authentication (EPA)

## References 

- Weak Security Controls and Practices Routinely Exploited for Initial Access | <https://www.cisa.gov/news-events/cybersecurity-advisories/aa22-137a>
- What is LLMNR Poisoning and How to Avoid It | <https://redfoxsec.com/blog/what-is-llmnr-poisoning-and-how-to-avoid-it/>
- How To Disable LLMNR & Why You Want To | <https://medium.com/@jamshed_hossain_miraz/how-to-disable-llmnr-why-you-want-to-e3f6deb9333d>
- How To Disable LLMNR & Why You Want To | <https://www.blackhillsinfosec.com/how-to-disable-llmnr-why-you-want-to/>
- How to disable NetBIOS and LLMNR Protocols via GPO in Windows 11/10 | <https://www.thewindowsclub.com/disable-netbios-and-llmnr-protocols-via-gpo>
- Link-local Multicast Name Resolution (LLMNR) | <https://www.rfc-editor.org/rfc/rfc4795>
