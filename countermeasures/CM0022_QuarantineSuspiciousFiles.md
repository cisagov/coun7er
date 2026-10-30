# Quarantine Suspicious Files

* **ID:** CM0022
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Eliminate
* **Status:** Active
* **Nice Work Role:** Incident Response (PD-WRL-003)
* **Skills & Knowledge:** Incident response; file triage and quarantine

## Intended Outcome

Quarantining suspicious files terminates adversary execution of malware.

## Introduction

Quarantining suspicious files involves relocating files to a location
where any harm that can be caused by malicious files is mitigated. This
will allow an organization to analyze suspicious files while mitigating
potential further harm to any other systems. 

An example of a security solution that can safely quarantine a file is Microsoft Defender, which can quarantine files automatically and manually depending on the configuration implemented.

## Preparation

- Determine a location where suspicious files can be quarantined to limit any harm that can be done by a malicious file. 
    - Many specialized software solutions that can safely quarantine a file offer options to specify where a quarantined file should be placed.
    - Some solutions only allow quarantining to a preselected, default location. 
    - Many antivirus/security solutions offer quarantine locations ready to be used without any configuration.
    - Quarantine environments are generally characterized by a storage location where files have strict permissions. 
    - Ensure access to the quarantine location is restricted to authorized personnel only.
- Determine a naming convention to help identify files and relevant information (date of detection, location origin, etc.).
- Configure security solutions so that only one security solution is leveraged to quarantine files to avoid conflicting attempts by different solutions to quarantine any file.
- If an Endpoint Detection and Response (EDR) or Antivirus (AV) is being used, the appropriate solution's documentation should be referenced to understand the methods implemented when quarantining a Potentially Unwanted Application (PUA)/Potentially Unwanted Program (PUP).
    - Understanding a security solution's implementation of file quarantining along with an understanding of a file's behavior is key to ensuring a file is quarantined properly.

## Risks

- This countermeasure can break legitimate functionality.
- False positives can result in quarantining a legitimate file.
- This countermeasure may not stop all ongoing activity.
- False negatives can result in a malicious file remaining outside of quarantine.
- Errors can render this countermeasure ineffective.
- Malicious files may escape quarantine and continue to operate.

## Guidance

- Moving suspicious files
    - Use AV/EDR solutions to manually flag or quarantine a suspicious file that were not marked automatically by the AV/EDR solution.
    - Relocate suspicious files to a location that has been secured and mitigates any potential harm that can be caused by malicious files.
- Logging relevant information
    - Log information related to any quarantined files (source of the file, flags that were raised, etc.) to aid in analysis efforts.
- Post-quarantine
    - Analyze suspicious files to determine whether they are benign or malicious. If a file cannot be determined to be benign or malicious after analysis efforts, a decision should be made to delete the file or release it from quarantine after considering the risks of either option.
    - If a file is determined to be malicious, it should be analyzed further to identify characteristics that can be leveraged to further fine-tune defenses and to defend against further compromise.
    - If a file has been confirmed to be benign, then it can be released from quarantine.

## Associated Techniques

- [T1036](https://attack.mitre.org/techniques/T1036) - Masquerading

## Related Countermeasures

- CM0003 - Verify/Configure File and Directory Permissions
- RM0066 - Vulnerable Software (*future*)
- CM0016 - Remove Suspicious Property List (PLIST) Files
- CM0021 - Enable Behavioral and Heuristic-based Malware Detection
- RM0185 - Block Container File Types at Email Gateways (*future*)
- CM0105 - Remove Malicious Enterprise Applications and Service Account Principals
- CM0065 - Isolate Endpoints from Network
- CM0110 - Detect Anti-Malware Scan Interface (AMSI) Bypass Attempts

## References

- Computer Security Incident Handling Guide | <https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf>
- Cybersecurity Incident & Vulnerability Response Playbooks | <https://www.cisa.gov/sites/default/files/2024-08/Federal_Government_Cybersecurity_Incident_and_Vulnerability_Response_Playbooks_508C.pdf>
- Take response actions on a file | <https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/respond-file-alerts>
- VirusTotal | <https://www.virustotal.com/gui/home/upload>
