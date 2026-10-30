# Prevent and Detect System Binary Proxy Execution

* **ID:** CM0091
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Infrastructure Support (PD-WRL-004)
* **Skills & Knowledge:** Windows security; system binaries, application control, and detection rules

## Intended Outcome

Preventing and detecting abuse of trusted system binaries restricts and detects adversary malicious code execution. 

## Introduction

Adversaries abuse trusted system binaries to execute malicious code by proxy to evade process and signature-based defenses.  Commonly abused system binaries include, but are not limited to rundll32, regsvr32, msbuild, msiexec, and mshta.    

## Preparation

-	Consult the references section for familiarization with system binaries that are commonly abused to achieve proxy execution.
-	Determine the necessity of permitting the execution of these binaries in the environment.  This can be laborious, but each individual binary should be assessed for the likelihood of causing an unintended interruption if it is blocked or disabled.   
-	To the extent possible, identify baseline behavior for binaries commonly used to achieve proxy execution.

## Risks

-	Blocking or disabling system binaries can break legitimate functionality.  Responders must take steps to mitigate the risk of operational disruption.
-	Monitoring binaries for proxy execution can lead to false positives.  Baselining and optimization can help reduce the likelihood of false positives.  

## Guidance

Remediating adversary proxy execution of signed and/or trusted binaries may require organizations make changes to  existing application control settings and host-based agents alerts to improve subsequent detection and prevention of this technique.

### Prevent

-	Configure the host-based agent or firewall to block binaries that are commonly used to achieve system proxy execution.    
-	Enable auditing for successful and unsuccessful attempts to abuse system binaries and ensure the effective logging and monitoring of this activity. 

### Detect

Configure the host-based agent to monitor for:

-   Process execution
    -  Process lineage to include, at a minimum, parent-child relationships
    -  Process metadata to detect binary masquerading
-   Command-line parameter inspection
-	DLL module loads
-	Network calls from binaries commonly used to achieve proxy execution

## Associated Techniques

-	[T1218](https://attack.mitre.org/techniques/T1218/) – System Binary Proxy Execution

## Related Countermeasures

- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0005 - Monitor or Block Microsoft HTML Application (MSHTA) Utility or Restrict HTML Application (HTA) Network Access
- CM0006 - Disable and Monitor Regasm.exe and Regsvcs.exe Utilities
- CM0007 - Disable and Monitor Verclsid.exe System Binary
- CM0008 - Block or Monitor Mavinject.exe Utility
- CM0046 - Block Execution of Compiled HTML (CHM) Files
- CM0057 - Verify Signing of AppleScripts

## References

- Living off the Land Binaries, Scripts and Libraries | <https://lolbas-project.github.io>
- Rundll32 | <https://redcanary.com/threat-detection-report/techniques/rundll32/>
