# Disable or Restrict Microsoft Build Engine (MSBuild.exe)

* **ID:** CM0130
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Examine
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows administration; MSBuild restrictions and application control

## Intended Outcome

Disabling Microsoft Build Engine (MSBuild) prevents an adversary's attempts to evade defenses and deliver malware using MSBuild. 

## Introduction

The Microsoft Build Engine (MSBuild.exe) is used to compile .NET code and other programs in the absence of the Visual Studio integrated development environment (IDE), as well as under the hood of Visual Studio itself. Beginning with Visual Studio 2019, some versions MSBuild Tools are considered obsolete if the IDE is present in the environment. 

Adversaries can use MSBuild to load fileless malware onto systems and evade detection. Since MSBuild is signed by Microsoft, attackers are using legitimate system applications to load and execute their malware, leaving no traces of initial compromise. Adversaries have used MSBuild for installation of remote access trojans (RATs), credential stealers, and Cobalt Strike beacons.   

## Preparation

- Identify how MSBuild is used in the organization. 
- Determine the expected use case of .NET code within the organization.
- Identify if and how Visual Studio is used in the environment. Identify roles that need to use Visual Studio to perform their job. 
- Ensure that the system is configured to properly log 'process creation' events. Enable the **Audit Process Creation** policy in the Group Policy Management Console.

## Risks

- MSBuild functionality is required for business operations in organizations where Visual Studio is used or .NET code needs to be built from source. Disabling MSBuild in these cases is detrimental. Consider restricting and monitoring MSBuild instead. 
- Renaming, changing, or moving MSBuild.exe may evade detection,  Applocker, or WDAC policies. Having several types of rules active simultaneously (e.g. a hash rule and a path rule) can mitigate some risk, since the rules protect against different behaviors.  

## Guidance

### Restricting MSBuild

- Restrict running MSBuild only to users and roles that need it to perform their jobs. 
	- Navigate to `Identity > Applications > Enterprise Applications > All applications` in the Microsoft Entra admin center.
	- Select Visual Studio. 
	- On the application's Overview page, navigate to `Manage > Properties` and enable the `Assignment required?` setting.
	- under `Manage`, navigate to `Users and groups > Add user/group > Users`. 
	- Select `None selected` and add users from the selector pane that opens. Select `Assign` to complete assignment. 

### Monitoring MSBuild

- Monitor all calls to `MSBuild.exe` via command line or PowerShell. The `Get-WinEvent` cmdlet can be leveraged to query the Windows Event Log for events related to the execution of MSBuild.exe.
- Monitor for .NET utility `dotnet build` which is a wrapper for MSBuild.  

If disabling is feasible, then MSBuild can be removed from the system or disabled via Applocker or Windows Defender Application Control (WDAC). Microsoft recommends this approach. 

### Disable via Applocker

- Implement Applocker's default allow rules regarding executables.
	- In the Group Policy Management Console (GPMC) navigate to `Security Settings > Application Control Policies > AppLocker > Executable Rules`.
	- Select `Executable Rules` and enable `Create Default Rules`.
- Define a deny rule for `MSBuild.exe`. 
- Alternatively, block all file paths where the MSBuild.exe executable is located.

### Disable via Windows Defender Application Control

WDAC can be configured for both allow and deny policies based on organizational needs. When creating deny policies, Microsoft requires the inclusion of "Allow All" rules in both kernel and user mode sections to avoid blocking all software (some explicitly and some implicitly, since there are no rules to allow it). Microsoft provides an AllowAll policy template. This template will not affect any explicit allow rules already in place.  

Policies can be created using PowerShell or via the WDAC wizard.

| Rule Type | PowerShell Command |
|-----------|--------------------|
| Software Publisher-based | `$DenyRules += New-CIPolicyRule -Level FilePublisher -DriverFilePath MSBuild.exe -Fallback SignedVersion,Publisher,Hash -Deny` |
| Software attribute-based | `$DenyRules += New-CIPolicyRule -Level FileName -DriverFilePath MSBuild.exe -Fallback Hash -Deny` |
| Hash-based | `$DenyRules += New-CIPolicyRule -Level Hash -DriverFilePath MSBuild.exe -Deny` | 

Merge the deny rules with the AllowAll template policy:
```
$DenyPolicy = <path_to_deny_policy_destination>
$AllowAllPolicy = $Env:windir + "\schemas\CodeIntegrity\ExamplePolicies\AllowAll.xml"
Merge-CIPolicy -PolicyPaths $AllowAllPolicy -OutputFilePath $DenyPolicy -Rules $DenyRules
Set-CiPolicyIdInfo -FilePath $DenyPolicy -PolicyName "Deny MSBuild" -ResetPolicyID
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

- [T1127.001](https://attack.mitre.org/techniques/T1127/001/) - Trusted Developer Utilities Proxy Execution: MSBuild

## Related Countermeasures

- CM0005 - Monitor or Block Microsoft HTML Application (MSHTA) Utility or Restrict HTML Application (HTA) Network Access
- CM0006 - Disable and Monitor Regasm.exe and Regsvcs.exe Utilities
- CM0007 - Disable and Monitor Verclsid.exe System Binary
- CM0008 - Block or Monitor Mavinject.exe Utility
- CM0101 - Block Applications in Writable Locations using AppLocker
- CM0117 - Disable or Monitor Connection Manager Profile Installer (CMSTP.exe)
- CM0127 - Disable Microsoft XML Core Services (MSXSL.exe)
- CM0131 - Disable or Restrict Microsoft Management Console (MMC.exe)
- CM0132 - Disable or Restrict InstallUtil

## References

- Hackers Using Microsoft Build Engine to Deliver Malware Filelessly |  <https://thehackernews.com/2021/05/hackers-using-microsoft-build-engine-to.html>
- MSBuild | <https://learn.microsoft.com/en-us/visualstudio/msbuild/msbuild?view=vs-2022>
- Applications that can bypass WDAC and how to block them | <https://learn.microsoft.com/en-us/windows/security/application-security/application-control/app-control-for-business/design/applications-that-can-bypass-appcontrol>
- Restrict a Microsoft Entra app to a set of users | <https://learn.microsoft.com/en-us/entra/identity-platform/howto-restrict-your-app-to-a-set-of-users>