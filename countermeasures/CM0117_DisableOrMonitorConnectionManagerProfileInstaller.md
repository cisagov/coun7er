# Disable or Monitor Connection Manager Profile Installer (CMSTP.exe)

* **ID:** CM0117
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; CMSTP monitoring and application control, AppLocker administration

## Intended Outcome

Disabling the Connection Manager Profile Installer (CMSTP.exe) prevents an adversary's attempts to evade defenses and achieve proxy execution using CMSTP.

## Introduction

The Connection Manager Profile Installer (CMSTP.exe) is a command-line program for installing Connection Manager service profiles from installation information files (INF). System administrators use this service to connect to remote networks via configured profiles. This software is installed on Windows endpoints by default and is signed by Microsoft. 

Adversaries can use the service to execute arbitrary code by providing INF files containing malicious commands, bypassing existing user account control.   

## Preparation

- Determine impacts that disabling the service will have on business operations. Identify connections that need to be installed using this service, such as virtual private networks (VPNs). 
- Identify any existing Group Policy Objects (GPOs) for software or executable restrictions. 
- If creating policies using the Windows Defender Application Control (WDAC) on versions of Windows Server 2022 or above, Windows 10 version 1903 and above, or Windows 11 versions 22H2 or below, ensure the WDAC policy refresh tool is available on all managed endpoints. 

## Risks

- Removing or disabling connection manager may be disruptive to business operations if the organization uses CMSTP.exe for remote network connections, for example over VPN.
- Creating and pushing application policy updates may be disruptive to business operations. Consider updating policies during slower hours. 
- Renaming, changing, or moving CMSTP.exe may evade Applocker or WDAC policies. Having several types of rules active simultaneously (e.g. a hash rule and a path rule) can mitigate some risk, since the rules protect against different behaviors. 

## Guidance

If CMSTP is not required for business operations of the organization, it should be disabled to lessen the attack surface and reduce the logging load on systems and analysts.

AppLocker is the recommended tool for disabling software alongside Windows Defender Application Control (WDAC) after Microsoft ended support for Software Restriction Policies via GPOs. Both AppLocker and WDAC are "deny by default, allow by exception" applications. 

### Restriction via AppLocker

- Implement Applocker's default allow rules regarding executables.
	- In the Group Policy Management Console (GPMC) navigate to `Security Settings > Application Control Policies > AppLocker > Executable Rules`.
	- Select `Executable Rules` and enable `Create Default Rules`.
- Define a deny rule for `cmstp.exe`. 
- Alternatively, block locations of the CMSTP.exe executable (all locations must be blocked). Take care not to block any legitimate applications during this process. 

### Restriction via Windows Defender Application Control

WDAC can be configured for both allow and deny policies based on organizational needs. When creating deny policies, Microsoft requires the inclusion of "Allow All" rules in both kernel and user mode sections to avoid blocking all software (some explicitly and some implicitly, since there are no rules to allow it). Microsoft provides an AllowAll policy template. This template will not affect any explicit allow rules already in place.  

Policies can be created using PowerShell or via the WDAC wizard.

| Rule Type | PowerShell Command |
|-----------|--------------------|
| Software Publisher-based | `$DenyRules += New-CIPolicyRule -Level FilePublisher -DriverFilePath cmstp.exe -Fallback SignedVersion,Publisher,Hash -Deny` |
| Software attribute-based | `$DenyRules += New-CIPolicyRule -Level FileName -DriverFilePath cmstp.exe -Fallback Hash -Deny` |
| Hash-based | `$DenyRules += New-CIPolicyRule -Level Hash -DriverFilePath cmstp.exe -Deny` | 

Merge the deny rules with the AllowAll template policy:
```
$DenyPolicy = <path_to_deny_policy_destination>
$AllowAllPolicy = $Env:windir + "\schemas\CodeIntegrity\ExamplePolicies\AllowAll.xml"
Merge-CIPolicy -PolicyPaths $AllowAllPolicy -OutputFilePath $DenyPolicy -Rules $DenyRules
Set-CiPolicyIdInfo -FilePath $DenyPolicy -PolicyName "Deny CMSTP" -ResetPolicyID
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
 
- Deployment can vary depending on the version of Windows a client is running and whether it is a server or workstation. More detailed steps, including commands, are included in the tutorial in the **References** section. 
- For Windows 11 22H2 and above:
	- Use the CiTool to apply policies

- For Windows 11, Windows 10 version 1903 and above, Windows Server 2022 and above:
	- Initialize variables.
	- Copy the policy binary to the destination folder with the `Copy-Item -Path $PolicyBinary -Destination $DestinationFolder -Force` command.
	- Run RefreshPolicy.exe to activate and refresh policies on a managed endpoint.

- All other versions of Windows and Windows Server:
	- Initialize variables.
	- Copy policy binary to destination folder with `Copy-Item  -Path $PolicyBinary -Destination $DestinationBinary -Force` command. 
	- Activate and refresh policies using Windows Management interface (WMI). 

### Monitoring CMSTP

If disabling the service is unfeasible in the environment, monitor any processes that call the executable.
- Monitor process command lines containing `cmstp.exe`.
- Monitor changes to directory where cmstp.exe is located, such as renaming or moving files. 

## Associated Techniques

- [T1218.003](https://attack.mitre.org/techniques/T1218/003/) - System Binary Proxy Execution: CMSTP 

## Related Countermeasures

- CM0005 - Monitor or Block Microsoft HTML Application (MSHTA) Utility or Restrict HTML Application (HTA) Network Access
- CM0006 - Disable and Monitor Regasm.exe and Regsvcs.exe Utilities
- CM0007 - Disable and Monitor Verclsid.exe System Binary
- CM0008 - Block or Monitor Mavinject.exe Utility
- CM0101 - Block Applications in Writable Locations using AppLocker
- CM0127 - Disable Microsoft XML Core Services (MSXSL.exe)
- CM0130 - Disable or Restrict Microsoft Build Engine (MSBuild.exe)
- CM0131 - Disable or Restrict Microsoft Management Console (MMC.exe)
- CM0132 - Disable or Restrict InstallUtil

## References

- AppLocker | <https://learn.microsoft.com/en-us/windows/security/application-security/application-control/windows-defender-application-control/applocker/applocker-overview>
- Application Control for Windows | <https://learn.microsoft.com/en-us/windows/security/application-security/application-control/windows-defender-application-control/design/select-types-of-rules-to-create#windows-defender-application-control-file-rule-levels>
- Guidance on Creating WDAC Deny Policies | <https://learn.microsoft.com/en-us/windows/security/application-security/application-control/windows-defender-application-control/design/create-wdac-deny-policy#creating-a-deny-policy-tutorial>
- Deploying Windows Defender Application Control (WDAC) policies | <https://learn.microsoft.com/en-us/windows/security/application-security/application-control/windows-defender-application-control/deployment/wdac-deployment-guide>
- How Connection Manager Works | <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc786431(v=ws.10)>
