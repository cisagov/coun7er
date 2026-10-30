# Reboot Servers

* **ID:** CM0026
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Server administration; service availability and safe reboot procedures

## Intended Outcome

Rebooting servers blocks adversary initial access using patched
vulnerabilities and restricts adversary execution of fileless malware.

## Introduction

No Introduction content identified.

## Preparation

- Establish which users need to be notified of reboots.
- Reboot systems at times that will minimize disruption.

## Risks

This countermeasure may have impacts that disrupt operations, including:

-   Downtime - Rebooting a server can result in downtime for the applications or services that are hosted on that server. This can impact the productivity of the organization and potentially lead to revenue losses.
-   Data loss - If a server is not properly backed up before rebooting, there is a risk of data loss if the server fails to restart properly or if there are any issues during the reboot process.
-   Application compatibility issues - Some applications may not be compatible with the latest software versions and security patches that are installed during the reboot process. This can result in application failures or compatibility issues that can impact the productivity of the organization.

This countermeasure may introduce vulnerabilities, including:

-   Configuration errors - In some cases, rebooting a server can result in configuration errors that can impact the functionality of the server or its applications. This can result in additional downtime and potentially require further troubleshooting and maintenance.
-   Security risks - Rebooting a server can temporarily disable security features such as firewalls or intrusion detection systems, which can leave the server vulnerable to attacks during the reboot process.
-   Preventing Recovery - If system(s) are infected with ransomware, rebooting a compromised system risks restarting tripped up/stuck ransomware or malware scheduled to activate on rebooting. Moreover, any artifacts saved to memory, such as potential decryption keys on some ransomware variants, will be lost of a system is rebooted. Rebooting the system may only be appropriate to clear memory resident malware that is unlikely to cause data loss.

## Guidance

- Schedule reboots during off-peak hours.
- Notify users and stakeholders in advance of any planned system reboots to minimize any potential disruptions or downtime.
- After the system has been rebooted, test all applications and services to ensure that they are functioning properly.
- Monitor the system after the reboot to ensure that it is running smoothly and that there are no issues or errors.
- Keep the system and its applications up to date with the latest software versions and security patches to minimize the risk of any compatibility issues or security vulnerabilities.

## Associated Techniques

- [T1563.002](https://attack.mitre.org/techniques/T1563/002) - Remote Service Session Hijacking: RDP Hijacking
- [T1563.001](https://attack.mitre.org/techniques/T1563/001) - Remote Service Session Hijacking: SSH Hijacking
- [T1592.002](https://attack.mitre.org/techniques/T1592/002) - Gather Victim Host Information: Software
- [T1592.003](https://attack.mitre.org/techniques/T1592/003) - Gather Victim Host Information: Firmware
- [T1592.004](https://attack.mitre.org/techniques/T1592/004) - Gather Victim Host Information: Client Configurations
- [T1595.002](https://attack.mitre.org/techniques/T1595/002) - Active Scanning: Vulnerability Scanning

## Related Countermeasures

- CM0066 - Update Firmware
- CM0067 - Install Critical Software Security Updates
- CM0086 - Reboot Workstations

## References

- Experts: Don't reboot your computer after you've been infected with ransomware | <https://www.zdnet.com/article/experts-dont-reboot-your-computer-after-youve-been-infected-with-ransomware/>
