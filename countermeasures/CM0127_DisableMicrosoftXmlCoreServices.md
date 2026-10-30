# Disable Microsoft XML Core Services (MSXSL.exe)

* **ID:** CM0127
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; MSXSL utilities and application execution controls, AppLocker administration

## Intended Outcome

Disabling Microsoft XML Core Services (MSXSL.exe) prevents an adversary's attempts at defense evasion and arbitrary code execution by abusing the MSXSL.exe utility. 

## Introduction

The MSXSL utility is an older Windows utility for converting files (usually XML spreadsheets) into other formats such as plaintext or HyperText Markup Language (HTML). An extensible stylesheet language transformation (XSLT) file provides the transformation specifications to the utility. MSXSL takes two arguments: a file to transform and a file with transformation instructions. 

Adversaries can abuse the utility to execute code containing an `ms:script` element, such as Visual Basic scripts, JScript, and Component Object Model (COM) objects. The file arguments can be URL links to adversary-owned infrastructure and are not required to be stored on disk. This may be used as a technique to bypass application controls, since adversaries only need to get a very small utility signed by Microsoft onto disk, and can then use that to load additional tools or establish persistence. 

## Preparation

- Identify if MSXSL is necessary for business operations.
- Identify any existing Group Policy Objects (GPOs) for software or executable restrictions. 
- If creating policies using the Windows Defender Application Control (WDAC) on versions of Windows Server 2022 or above, Windows 10 version 1903 and above, or Windows 11 versions 22H2 or below, ensure the WDAC policy refresh tool is available on all managed endpoints.  

## Risks

- Logging process command lines may be logging-intensive. Consider scope and filters for logging and ensure that proper space is allotted to logs. 
- Renaming, changing, or moving MSXSL.exe may evade detection,  Applocker, or WDAC policies. Having several types of rules active simultaneously (e.g. a hash rule and a path rule) can mitigate some risk, since the rules protect against different behaviors. 

## Guidance

If disabling msxls.exe is not feasible in the environment, instead restrict and monitor all calls to the executable. An adversary may install or bring msxsl.exe with them to achieve proxy execution even if the binary wasn't originally in the environment. Keeping detection rules active even after removing the binary can help alert on future recompromises via MSXSL. If msxsl.exe is not used by the organization or not critical to business operations, disable and remove it. 

### Remove the utility

If feasible for the organization, MSXSL can be uninstalled from endpoints via control panel. 

### Disable using Applocker

- Implement Applocker's default allow rules regarding executables.
	- In the Group Policy Management Console (GPMC) navigate to `Security Settings > Application Control Policies > AppLocker > Executable Rules`.
	- Select `Executable Rules` and enable `Create Default Rules`.
- Define a deny rule for `msxls.exe`. 
- Alternatively, block locations of the MSXSL.exe executable (all locations must be blocked). Take care not to block any legitimate applications during this process. 

### Disable using Windows Defender Application Control

WDAC can be configured for both allow and deny policies based on organizational needs. When creating deny policies, Microsoft requires the inclusion of "Allow All" rules in both kernel and user mode sections to avoid blocking all software (some explicitly and some implicitly, since there are no rules to allow it). Microsoft provides an AllowAll policy template. This template will not affect any explicit allow rules already in place.  

Policies can be created using PowerShell or via the WDAC wizard.

| Rule Type | PowerShell Command |
|-----------|--------------------|
| Software Publisher-based | `$DenyRules += New-CIPolicyRule -Level FilePublisher -DriverFilePath msxls.exe -Fallback SignedVersion,Publisher,Hash -Deny` |
| Software attribute-based | `$DenyRules += New-CIPolicyRule -Level FileName -DriverFilePath msxls.exe -Fallback Hash -Deny` |
| Hash-based | `$DenyRules += New-CIPolicyRule -Level Hash -DriverFilePath msxls.exe -Deny` | 

Merge the deny rules with the AllowAll template policy:
```
$DenyPolicy = <path_to_deny_policy_destination>
$AllowAllPolicy = $Env:windir + "\schemas\CodeIntegrity\ExamplePolicies\AllowAll.xml"
Merge-CIPolicy -PolicyPaths $AllowAllPolicy -OutputFilePath $DenyPolicy -Rules $DenyRules
Set-CiPolicyIdInfo -FilePath $DenyPolicy -PolicyName "Deny MSXSL" -ResetPolicyID
```

Deploy WDAC policies:

- Convert XML policy into binary. An example PowerShell command to do so is listed below. Ensure that the `$WDACPolicyXMLFile` variable points to the location of the XML policy file. 
    ```
    ## Update the path to your WDAC policy XML
     $WDACPolicyXMLFile = $env:USERPROFILE + "\Desktop\MyWDACPolicy.xml"
     [xml]$WDACPolicy = Get-Content -Path $WDACPolicyXMLFile
     if (($WDACPolicy.SiPolicy.PolicyID) -ne $null) ## Multiple policy format (For Windows builds 1903+ only, including Server 2022)
     {
         $PolicyID = $WDACPolicy.SiPolicy.PolicyID
         $PolicyBinary = $PolicyID+".cip"
     }
     else ## Single policy format (Windows Server 2016 and 2019, and Windows 10 1809 LTSC)
     {
         $PolicyBinary = "SiPolicy.p7b"
     }
    ## Binary file will be written to your desktop
     ConvertFrom-CIPolicy -XmlFilePath $WDACPolicyXMLFile -BinaryFilePath $env:USERPROFILE\Desktop\$PolicyBinary
     ```
 
Deployment can vary depending on the version of Windows a client is running and whether it is a server or workstation. 

- For Windows 11 22H2 and above:
	- Use the CiTool to apply policies.
- For Windows 11, Windows 10 version 1903 and above, Windows Server 2022 and above:
	- Initialize variables.
	- Copy the policy binary to the destination folder.
	- Run RefreshPolicy.exe to activate and refresh policies on a managed endpoint.
- All other versions of Windows and Windows Server:
	- Initialize variables.
	- Copy policy binary to destination folder. 
	- Activate and refresh policies using Windows Management Interface (WMI).  

### Monitor MSXSL.exe

If MSXSL is commonly used in the environment, examine common precursor or following commands to `msxsl.exe` to create exclusion rules or lower priority alerts for legitimate behavior.

- Monitor all calls to msxsl.exe not containing the `-o` flag.
- Monitor execution of msxsl.exe where a valid URL or IP address is in the command line instead of a local file name.

## Associated Techniques

- [T1220](https://attack.mitre.org/techniques/T1220/) - XSL Script Processing
- [T1059](https://attack.mitre.org/techniques/T1059/) - Command and Scripting Interpreter

## Related Countermeasures

- CM0005 - Monitor or Block Microsoft HTML Application (MSHTA) Utility or Restrict HTML Application (HTA) Network Access
- CM0006 - Disable and Monitor Regasm.exe and Regsvcs.exe Utilities
- CM0007 - Disable and Monitor Verclsid.exe System Binary
- CM0008 - Block or Monitor Mavinject.exe Utility
- CM0101 - Block Applications in Writable Locations using AppLocker
- CM0117 - Disable or Monitor Connection Manager Profile Installer (CMSTP.exe)
- CM0130 - Disable or Restrict Microsoft Build Engine (MSBuild.exe)
- CM0131 - Disable or Restrict Microsoft Management Console (MMC.exe)
- CM0132 - Disable or Restrict InstallUtil

## References

- Detecting MSXSL Abuse in the Wild | <https://redcanary.com/blog/threat-detection/detecting-msxsl-attacks/>
- MSXSL Security Overview | <https://learn.microsoft.com/en-us/previous-versions/windows/desktop/ms754611(v=vs.85)>
- MSXSL.EXE AND WMIC.EXE — A Way to Proxy Code Execution | <https://medium.com/@threathuntingteam/msxsl-exe-and-wmic-exe-a-way-to-proxy-code-execution-8d524f642b75>