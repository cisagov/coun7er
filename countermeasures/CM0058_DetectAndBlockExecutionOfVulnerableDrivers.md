# Detect and Block Execution of Vulnerable Drivers

* **ID:** CM0058
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine, Disable
* **Status:** Active
* **Nice Work Role:** Infrastructure Support (PD-WRL-004)
* **Skills & Knowledge:** Endpoint security; driver management and vulnerable-driver blocking

## Intended Outcome

Detecting and blocking the execution of vulnerable drivers prevents and/or detects adversaries attempting to use vulnerable drivers to evade defenses and escalate privileges. 

## Introduction

Drivers are software that act as intermediaries between the operating system and the underlying hardware. In essence, a driver solves compatibility issues between hardware and software. Adversaries abuse vulnerable drivers using a technique known as Bring Your Own Vulnerable Driver (BYOVD). In this attack an adversary delivers and installs a vulnerable driver on to the target system and exploits the vulnerability to execute code in kernel mode. Adversaries in the past or targeting older platforms would load unsigned drivers for exploitation. Due to Windows Driver Signature Enforcement (DSE), only signed kernel drivers can be loaded. Therefore, adversaries will load and exploit vulnerable signed drivers and then load unsigned malicious drivers. Microsoft's attack surface reduction (ASR) tool, Intune, can be configured to prevent installation of vulnerable signed drivers. 

## Preparation

- Consult documentation and SMEs regarding needed drivers. 

## Risks

- Blocking vulnerable drivers can result in operational disruption if hardware that is no longer supported is in use. 
- Applications with embedded device drivers will break or lose functionality. Check vendor to see is a solution for decoupling an application from the embedded driver with a more secure version. 
- Blocking vulnerable device drivers can cause Windows to produce a "Blue Screen of Death" error. 

## Guidance

### Detection of vulnerable or malicious drivers

* Monitor driver loads through event logs. 
    - Sysmon event id 6 records driver loads.
* Monitoring Services
    - Command line monitoring (sc.exe query)
    - Reg modifications to HKLM\SYSTEM\CurrentControlset\Services
    - Manual inspection using Windows Device Manager (devmgmt.msc)
* Alert to security processes terminated by drivers
* Audits using specialized software, driver queries, or manual inspection and comparing active driver list to a whitelist or blacklist of drivers. 

### Blocking execution or loading of vulnerable or malicious drivers

To address the challenge of adversaries loading signed drivers, the certificate of known vulnerable or malicious drivers should be in a revocation list. 
Microsoft provides tools which can be enabled or configured to prevent abuse of vulnerable drivers which will be covered in this guidance section. 

#### Enable/Verify Microsoft's vulnerable driver blocklist

Microsoft's vulnerable driver blocklist is a native utility for Windows 11 2022 and above that receives updates 1-2 times per year for the block list. Vulnerable driver blocklist is enforced when Hypervisor-protected coded integrity or HVCI, Smart App Control, or S mode is active. 
Navigate to the Windows Security App -> Click Device Security -> Core Isolation -> Toggle switch on for Microsoft Vulnerable Driver Blocklist if off.

#### Windows Defender Application Control (WDAC)

For devices where HVCI or S mode is not available, Microsoft offers a recommended block list that can be added to Windows Defender Application Control policy. To view and download list, search for Microsoft's driver block list. It is also recommended to test the policy in audit mode and review the audit block events to mitigate implementation risks. 

## Associated Techniques       

- [T1068](https://attack.mitre.org/techniques/T1068/) - Exploitation for Privilege Escalation
- [T1553.002](https://attack.mitre.org/techniques/T1553/002) - Subvert Trust Controls: Code Signing
- [T1562.001](https://attack.mitre.org/techniques/T1562/001/) - Impair Defenses: Disable or Modify Tools
- [T1562.010](https://attack.mitre.org/techniques/T1562/010/) - Impair Defenses: Downgrade Attack

## Related Countermeasures

- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- RM0229 - Validate Driver Integrity (*future*)
- CM0034 - Identify and Block Suspicious Hosts

## References

- Should You Use Microsoft Vulnerable Driver Blocklist? | <https://www.itprotoday.com/vulnerabilities-threats/should-you-use-microsoft-vulnerable-driver-blocklist->
- What is a driver? | <https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/what-is-a-driver->
- Strategies to monitor and prevent vulnerable driver attacks | <https://techcommunity.microsoft.com/t5/microsoft-security-experts-blog/strategies-to-monitor-and-prevent-vulnerable-driver-attacks/ba-p/4103985#:~:text=This%20involves%20introducing%20a%20digitally,system%20behaviour%20to%20remain%20undetected.>
- Microsoft recommended driver block rules | <https://learn.microsoft.com/en-us/windows/security/application-security/application-control/windows-defender-application-control/design/microsoft-recommended-driver-block-rules>
- SCATTERED SPIDER Exploits Windows Security Deficiencies with Bring-Your-Own-Vulnerable-Driver Tactic in Attempt to Bypass Endpoint Security | <https://www.crowdstrike.com/en-us/blog/scattered-spider-attempts-to-avoid-detection-with-bring-your-own-vulnerable-driver-tactic/>
