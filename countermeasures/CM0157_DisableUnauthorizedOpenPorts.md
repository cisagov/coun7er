# Disable Unauthorized Open Ports

* **ID:** CM0157
* **Version:** 0.1
* **Created:** 21 January 2025
* **Modified:** 04 March 2025
* **Countermeasure Type:** Disable
* **Status:** Active
* **Nice Work Role:** Network Operations (IO-WRL-004)
* **Skills & Knowledge:** Network administration; open-port auditing and unauthorized service restrictions

## Intended Outcome

Disabling unauthorized open ports prevents adversaries from establishing persistence, lateral movement, and command and control foothold via the setup of unauthorized services, backdoors, or exfiltration channels on unused or high-numbered ports.

## Introduction

In order to evade detection by modern network monitoring solutions, adversaries may establish unauthorized services on high and uncommonly used ports. Adversaries may engage services such as HTTPS over an uncommon port to facilitate command and control (C2) communication and data exfiltration. For this reason, organizations must regularly audit open ports. Attackers often exploit misconfigurations or weak firewall policies to run services on non-standard ports.

## Preparation

- Maintain and frequently reference an inventory of authorized services and ports required for business operations.
- Enable logging and monitoring of open ports across all endpoints, servers, and perimeter firewalls.
- Define anomalous port activity thresholds from what is expected within business operations.
- Identify open ports by conducting regular network scans using tools `nmap`, `Netstat`, or automated SIEM functions.
## Risks

- Enforcing restrictive port-blocking policies may inadvertently block legitimate business applications.
- Disabling open ports outright may interrupt administrative activity that temporarily utilizes high ports to perform network diagnosis processes. For example, a non-malicious service or program such as Splunk can stop functioning as a result of disabling ports used as part of an update sequence.


## Guidance

### Identify Unauthorized Open Ports

- Conduct regular network scans using tools such as `nmap` and `netstat.` 
	- `netstat -ano` and `lsof -i` are two commands that can be executed to determine which applications or users are opening unexpected ports.
- Compare discovered open ports against an authorized services list to detect deviations in port usage that spans normal business operations.
- Ensure investigation of the high port range (49152 - 65535). While this range is used for dynamic and private ports, it is often culprit for hidden adversarial services.
- Immediately disable or isolate systems found running unauthorized services to prevent proliferation of adversary activity within the network.

### Disable Ports in Windows Firewall 

- Port management is governed by Windows Firewall rules for Windows systems. The following guidance is relevant to Windows 10 and Windows Server 2016 operating systems:
	1. Navigate to 'System and Security' from the Windows Control Panel.
	2. Navigate 'Windows Defender Firewall' > 'Advanced Settings' > Select Outbound/Inbound Rules > Right-click 'New Rule'
	3. `Rule Type:` Choose 'Port' when selecting a rule type to be created.
	4. `Protocol and Ports:` Choose 'TCP' or 'UDP' to define the connection to control > Enter a singular port number, or range to be affected.
	5. `Action:` Select 'Block the connection' radio button.
	6. `Profile:` Select 'Domain,' 'Private,' and 'Public' checkboxes to disable port activity across all connections made by the computer.

- High-numbered ports (existing within a range above 49152) require special investigation to determine if they are involved with hidden service usage.
	- If high-numbered, dynamic ports are not utilized as part of business operations, implement firewall rules to block traffic to and from the high port range (Port 49152 to Port 65535).

- When determining specific ports to disable with Firewall rules, pay careful attention to commonly abused ports:
	- FTP (Port 20 and 21): Insecure and outdated-lacks encryption for data transfer or authentication.
	- SSH (Port 22): TCP port exploited by threat actors to gain unauthorized access to a system.
	- Telnet (Port 23): Vulnerable to spoofing, malware, credential brute-forcing, and credential sniffing.
	- NetBIOS (Port 139): Legacy printer/file sharing mechanism abused by attackers to discover IP addresses and session information.
	- Remote Desktop (Port 3389): RDP is a common target of session hijacking attempts.

### Disable Ports in Linux/Unix Systems

- In Linux/Unix Operating systems, port management is commonly performed using firewall services such as 'UFW' or 'iptables' in order to block incoming connections to a specific port number:
    1. Identify the service currently running on a port using commands such as `netstat -tulpn,` `ss -tln,` `ss -uln,` and `lsof -i -P -n`.
    2. Once a port number and its corresponding service is identified, the UFW (Uncomplicated Firewall) service can be utilized to supply a command `sudo ufw deny <port_number>` to block the port.
        - Ex: Executing `sudo ufw deny 80` will block all incoming traffic to port 80, which is commonly associated with the HTTP service.
    3. In an instance where a service is actively using a port as opposed to listening, the service can be stopped using `sudo systemctl stop <service_name>` to consequently close the port the service was running on.
    4. If more granular control over firewall rules is desired, the 'iptables' service can be utilized in the following fashion to block connections to a port number:
          - `sudo iptables -A INPUT -p tcp --dport 22 -m state --state NEW, ESTABLISHED -j DROP` 
       - Executing this command will assign a new rule to the INPUT chain handled by iptables, specifying that incoming TCP traffic matching packets of the NEW or ESTABLISHED connection states should be dropped. This effectively blocks both new and outgoing SSH connections.
            
        
## Associated Techniques

- [T1571](https://attack.mitre.org/techniques/T1571/) - Non-Standard Port
- [T1205](https://attack.mitre.org/techniques/T1205/) - Traffic Signaling

## Related Countermeasures

- RM0057 - Block Windows Management Instrumentation (WMI) Service
- RM0072 - Enable Port Filtering on Host-based Firewalls
- RM0109 - Disable NetBIOS Service

## References

- How to Open and Block Ports in Windows Firewall | <https://www.hostwinds.com/tutorials/how-to-open-or-block-ports-using-windows-firewall>

- Identifying Open Ports in Your Network Perimeter | <https://bluegoatcyber.com/blog/identifying-open-ports-in-your-network-perimeter/>

- Handling Open Ports Secure and Finding Vulnerabilities | <https://blog.netwrix.com/2022/08/16/open-network-ports/>

- ss Command in Linux with Examples | <https://linuxblog.io/ss-command-in-linux-with-examples/#:~:text=The%20ss%20command%20stands%20for,connections%2C%20and%20monitoring%20network%20traffic>

- Determining if a port is in use by an application or process on a virtual machine | <https://knowledge.broadcom.com/external/article?articleNumber=308682>
