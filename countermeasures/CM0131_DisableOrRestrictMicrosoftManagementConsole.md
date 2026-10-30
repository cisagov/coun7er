# Disable or Restrict Microsoft Management Console (MMC.exe)

* **ID:** CM0131
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; Microsoft Management Console and console file restrictions

## Intended Outcome

Disabling the Microsoft Management Console prevents an adversary's attempts at remote code execution and defense evasion using malicious management saved console files. 

## Introduction

The Microsoft Management Console (MMC) creates, saves, and opens administrative tools in the environment to manage the hardware, software, and network components of a Windows operating system. System administrators can use it to create custom tools and distribute them to users, alongside managing the OS. 

Adversaries can abuse the MMC by crafting malicious management saved console (MSC) files that allow them to execute arbitrary code, including malware payloads. Adversaries can use this technique to compromise systems and evade security defenses. Similar techniques have become popular after Microsoft restricted macros and add-ins by default.   

## Preparation

- Identify if MMC is used in the environment and the ramifications of disabling it. 
- Identify Windows Defender rules regarding MSC files. 

## Risks

- If the management console is required for organizational needs, disabling it will be disruptive. Consider instead auditing any Windows Defender policies and detection rules regarding MSC files. 
- Renaming, changing, or moving MMC.exe may evade detection,  Applocker, or Windows Defender Application Control (WDAC) policies. Having several types of rules active simultaneously (e.g. a hash rule and a path rule) can mitigate some risk, since the rules protect against different behaviors. 

## Guidance

### Restricting Microsoft Management Console and MMC snap-ins

- Create a new Group Policy Object (GPO) or locate an existing GPO regarding MMC.
- In the Group Policy Management Console, navigate to `User configuration > Administrative Templates > Windows Components > Microsoft Management Console` and enable the `Restrict users to the explicitly permitted list of snap-ins` setting.
- In a separate GPO, navigate to `User configuration > Administrative Templates > Windows Components > Microsoft Management Console > Restricted/Permitted snap-ins`.
- Allow any necessary snap-ins. All others will be restricted and inaccessible to users.
- Update group policy in the domain using `gpupdate --force`. 

### Monitoring Microsoft Management Console

- Monitor for calls to `mmc.exe` or `mmc`.
- Monitor for processes that take arguments with the .msc file extension.
- Monitor changes to the GPO policy list of allowed snap-ins. 

If not necessary in the environment, MMC can be uninstalled via the Installation Manager or disabled. One option is to disable MMC via GPOs by not specifying any allowed snap-ins. Other options include Applocker or Windows Defender Application Control. 

### Disabling via Applocker

- Implement Applocker's default allow rules regarding executables.
	- In the Group Policy Management Console (GPMC) navigate to `Security Settings > Application Control Policies > AppLocker > Executable Rules`.
	- Select `Executable Rules` and enable `Create Default Rules`.
- Define a deny rule for `MMC.exe`. 
- Alternatively, block locations of the MSBuild.exe executable.

### Disabling via Windows Defender Application Control (WDAC)

WDAC can be configured for both allow and deny policies based on organizational needs. When creating deny policies, Microsoft requires the inclusion of "Allow All" rules in both kernel and user mode sections to avoid blocking all software (some explicitly and some implicitly, since there are no rules to allow it). Microsoft provides an AllowAll policy template. This template will not affect any explicit allow rules already in place.  

Policies can be created using PowerShell or via the WDAC wizard.

| Rule Type | PowerShell Command |
|-----------|--------------------|
| Software Publisher-based | `$DenyRules += New-CIPolicyRule -Level FilePublisher -DriverFilePath MMC.exe -Fallback SignedVersion,Publisher,Hash -Deny` |
| Software attribute-based | `$DenyRules += New-CIPolicyRule -Level FileName -DriverFilePath MMC.exe -Fallback Hash -Deny` |
| Hash-based | `$DenyRules += New-CIPolicyRule -Level Hash -DriverFilePath MMC.exe -Deny` | 

Merge the deny rules with the AllowAll template policy:
```
$DenyPolicy = <path_to_deny_policy_destination>
$AllowAllPolicy = $Env:windir + "\schemas\CodeIntegrity\ExamplePolicies\AllowAll.xml"
Merge-CIPolicy -PolicyPaths $AllowAllPolicy -OutputFilePath $DenyPolicy -Rules $DenyRules
Set-CiPolicyIdInfo -FilePath $DenyPolicy -PolicyName "Deny MMC" -ResetPolicyID
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

## Associated Techniques

- [T1218.014](https://attack.mitre.org/techniques/T1218/014/) - System Binary Proxy Execution: MMC

## Related Countermeasures

- CM0005 - Monitor or Block Microsoft HTML Application (MSHTA) Utility or Restrict HTML Application (HTA) Network Access
- CM0006 - Disable and Monitor Regasm.exe and Regsvcs.exe Utilities
- CM0007 - Disable and Monitor Verclsid.exe System Binary
- CM0008 - Block or Monitor Mavinject.exe Utility
- CM0101 - Block Applications in Writable Locations using AppLocker
- CM0117 - Disable or Monitor Connection Manager Profile Installer (CMSTP.exe)
- CM0127 - Disable Microsoft XML Core Services (MSXSL.exe)
- CM0130 - Disable or Restrict Microsoft Build Engine (MSBuild.exe)
- CM0132 - Disable or Restrict InstallUtil

## References

- New Attack Technique Exploits Microsoft Management Console Files | <https://thehackernews.com/2024/06/new-attack-technique-exploits-microsoft.html>
- What is Microsoft Management Console? | <https://learn.microsoft.com/en-us/troubleshoot/windows-server/system-management-components/what-is-microsoft-management-console>
- AppLocker | <https://learn.microsoft.com/en-us/windows/security/application-security/application-control/windows-defender-application-control/applocker/applocker-overview>
- Application Control for Windows | <https://learn.microsoft.com/en-us/windows/security/application-security/application-control/windows-defender-application-control/design/select-types-of-rules-to-create#windows-defender-application-control-file-rule-levels>
- Guidance on Creating WDAC Deny Policies | <https://learn.microsoft.com/en-us/windows/security/application-security/application-control/windows-defender-application-control/design/create-wdac-deny-policy#creating-a-deny-policy-tutorial>
- Deploying Windows Defender Application Control (WDAC) policies | <https://learn.microsoft.com/en-us/windows/security/application-security/application-control/windows-defender-application-control/deployment/wdac-deployment-guide>
