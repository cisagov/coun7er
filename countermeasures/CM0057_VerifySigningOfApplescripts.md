# Verify Signing of AppleScripts

* **ID:** CM0057
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** macOS administration; AppleScript and code signing

## Intended Outcome 

Verifying that AppleScripts are signed by a trusted developer ID restricts an adversary's ability to arbitrarily execute untrusted AppleScripts.

## Introduction 

AppleScript is a scripting language native to MacOS. It can be abused in a manner similar to Bash, Python, and PowerShell. Adversaries have used AppleScript to achieve browser hijacking, spoofing, and persistence.

AppleScript can be used to achieve persistence, execution, and defense evasion, It can also be used to enable tactics like command-and-control. 

Verifying AppleScript signatures indicates that the AppleScript is from a trusted source. This measure will hinder an adversary's ability to abuse AppleScript to persist and further compromise the environment.

## Preparation 

- Identify and document all AppleScripts currently in use within the organization. Assess their source and establish inventory count on those that are signed and those unsigned.
- Compile a list of trusted developers whose signed scripts will be allowed to execute within the network. This list should be approved by the organization's security team.
- Define guidelines for corporate development of AppleScripts- specify requirements for script signing and testing before deployment.

## Risks 

- This countermeasure can interfere with business operations and efficacy as restricting AppleScripts to only those signed by trusted developers can negate the effect of unsigned scripts that existed in previous workflows.

## Guidance 

### Enforcing Script Signing Policies 

- Utilize macOS Gatekeeper to constrain script execution to signed scripts. Configure Gatekeeper to block unsigned scripts and those not signed by previously identified trusted developers.
- Conduct regular audits of script usage and signing policies to ensure that they remain effective and updated with the latest security practices and standards.

### Developer Verification Process

- Ensure that scripts are signed with a certificate that is validated/trusted by the organization.
- Ensure that certificates used for signing have not been revoked; ensure the integrity of the trusted developer list is up to date with certificate lifespan.

### Monitor for AppleScript Abuse

- Implement endpoint detection and response (EDR) tools to collect data on process creation events that suggest AppleScript execution. Look for processes like `osascript` and investigate any anomalies.
- Monitor the loading of modules and libraries related to AppleScript execution. Pay attention to any unsigned or suspicious modules.
- Regularly analyze logs from macOS systems to detect unusual AppleScript execution or access attempts.

## Associated Techniques 

- [T1059.002](https://attack.mitre.org/techniques/T1059/002/) - Command and Scripting Interpreter: AppleScript

## Related Countermeasures 

- CM0031 - Monitor PowerShell Program
- CM0091 - Prevent and Detect System Binary Proxy Execution

## References 

- How AppleScript Is Used For Attacking macOS | <https://www.sentinelone.com/blog/how-offensive-actors-use-applescript-for-attacking-macos/>
- Introduction to AppleScript Language Guide | <https://developer.apple.com/library/archive/documentation/AppleScript/Conceptual/AppleScriptLangGuide/introduction/ASLR_intro.html>
- Gatekeeper and runtime protection in macOS | <https://support.apple.com/guide/security/gatekeeper-and-runtime-protection-sec5599b66df/web>
