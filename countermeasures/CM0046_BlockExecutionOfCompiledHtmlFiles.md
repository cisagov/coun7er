# Block Execution of Compiled HTML (CHM) Files

* **ID:** CM0046
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; CHM files and application execution controls

## Intended Outcome

Blocking execution of compiled HTML (CHM) files blocks adversary defense evasion using proxy execution of malicious CHM files.

## Introduction

CHM files are compressed compilations of content such as HTML documents, images, and scripting/web related programming languages such VBA, JScript, Java, and ActiveX. The Microsoft proprietary online help format uses CHM files, and CHM files are sometimes used for e-books. CHM content is displayed on Windows machines using the HTML Help executable (hh.exe). Adversaries can abuse vulnerabilities in CHM files to execute arbitrary code by tricking users into opening or decompiling a malicious CHM file.

## Preparation

- Assess the impact to operations if users lose access to Microsoft HTML Help.

## Risks

- Blocking execution of all CHM files will eliminate Microsoft online help.

## Guidance

CHM file execution can be blocked in two primary ways. The first blocks execution of all CHM files; the second blocks execution of CHM files from Internet sources. The methods should be used in combination for maximum security.

### Block execution of hh.exe

Either of these methods will prevent the execution of CHM files in Windows.
- Use application control, such as EDR rules, to block execution of hh.exe. 
- Remove hh.exe from all client and server machines.

### Block CHM file download

- Restrict web-based traffic to block download of CHM files.
- Filter email attachments to remove CHM files. (Microsoft now blocks CHM files in its Outlook email client.)
- Configure web browsers so that they won't open CHM files.
- The .chm file extension can be used to identify CHM files. However, any file with CHM formatting, regardless of file extension, should be blocked. (One characteristic is that CHM files start with the ASCII bytes "ITSF.")
- Educate users so they know that .chm files are executable binary files and can cause the same harm as malicious .exe files.

## Associated Techniques 

- [T1218.001](https://attack.mitre.org/techniques/T1218/001/) - System Binary Proxy Execution: Compiled HTML File
- [T1204](https://attack.mitre.org/techniques/T1204/) - User Execution

## Related Countermeasures

- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0005 - Monitor or Block Microsoft HTML Application (MSHTA) Utility or Restrict HTML Application (HTA) Network Access
- CM0006 - Disable and Monitor Regasm.exe and Regsvcs.exe Utilities
- CM0007 - Disable and Monitor Verclsid.exe System Binary
- CM0008 - Block or Monitor Mavinject.exe Utility
- CM0091 - Prevent and Detect System Binary Proxy Execution

## References

- Microsoft Compiled HTML Help | <https://en.wikipedia.org/wiki/Microsoft_Compiled_HTML_Help>
- CHM | <https://www.trendmicro.com/vinfo/us/security/definition/chm>
- Cryptowall Makes a Comeback Via Malicious Help Files (CHM) | <https://www.bitdefender.com/en-us/blog/hotforsecurity/cryptowall-makes-a-comeback-via-malicious-help-files-chm/>
- Microsoft help file vulnerability could increase impact of phishing attack for all Windows users | <https://www.comparitech.com/blog/information-security/malicious-chm-files/>
