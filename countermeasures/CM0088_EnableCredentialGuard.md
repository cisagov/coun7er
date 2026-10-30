# Enable Credential Guard

* **ID:** CM0088
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Windows endpoint security; Credential Guard and virtualization-based security

## Intended Outcome

Enabling Credential Guard restricts adversary credential access.  

## Introduction

Credential Guard is one of several countermeasures available to prevent credential access and is enabled by default on Windows 11 22H2 and later.  Credential Guard uses Virtualization-based security (VBS) to isolate credentials so that only privileged system software can access them.  This countermeasure protects NTLM password hashes, Kerberos Ticket Granting Ticket (TGT), and credential manager domain credentials.    

## Preparation

-	For Credential Guard to be enabled, the following requirements must be met:
    -    Windows 11 version 22H2 or later
    -    64-bit CPU
    -    Support for Virtualization-based security
    -    Secure boot
-	Check for the existence of legacy authentication protocols.  Credential Guard does not permit the following protocols:
    -    NTLMv1
    -    Digest
    -    CredSSP
    -    MS-CHAPv2
    -    Kerberos unconstrained delegation
    -    Kerberos DES encryption support
    -    Extracting the Kerberos TGT
-	Check for the presence of 3rd party Security Support Providers (SSPs).  Some third-party SSPs may not be compatible with Credential Guard.  
-	The following features are not requirements but can provide additional protections:
    -    Trusted Platform Module (TPM) provides binding to hardware.
    -    UEFI lock prevents attackers from disabling Credential Guard.
- Verify status of Credential Guard. Administrators can verify whether Credential Guard is running on a system with the following PowerShell command `Get-CimInstance -ClassName 'Win32_DeviceGuard' -Namespace 'root\Microsoft\Windows\DeviceGuard'`

## Risks

-	Due to hardware and software requirements, there is a risk of operational disruption during deployment and performance degradation on systems with processors that do not support VBS. To minimize the likelihood of this risk, consider deploying Credential Guard in a phased approach and monitor audit logs to help ensure successful deployment.  
-	Credential Guard is not compatible with legacy authentication protocols and will prevent authentication or force users to manually authenticate if legacy protocols are in use.   
-	Third-party authentication modules must be digitally signed with a Microsoft digital signature and be compliant with the Microsoft Security Development Lifecycle.  If the third-party modules do not meet these requirements, there is a risk of operational disruption. 
-	Credential Guard can be disabled by privileged users. Disabling Credential Guard would not afford access to credential material in LSASS but would afford access to hashes from future logins.   
-	Credential Guard does not protect:
    -    Local accounts and Microsoft accounts
    -    Active Directory database on Windows Server domain controllers
    -    Keylogging
    -    Physical attacks
    -    The use of privileges associated with any credential
    -    3rd party security packages
    -    Supplied credentials for NTLM authentication
    -    Kerberos service tickets
    -    Windows cached credentials

## Guidance

### Enable Credential Guard with Group Policy Editor

-	Use the Group Policy Editor to create a new Group Policy Object (GPO) that’s linked to the domain or Organization Unit (OU) and enable Virtualization-based security.
-	Specify the Platform Security Level and Credential Guard Configuration. System administrators should select Secure Boot with DMA protection unless CPU does not support input/output memory management unit (in which case, select Secure Boot) and Enabled with UEFI lock to help ensure Credential Guard cannot be remotely disabled.
-	Apply the policy and verify that it was successfully enabled.

### Enable Credential Guard with Microsoft Intune

- Alternately, system administrators can enable Credential Guard via the Microsoft Intune admin center. 
- Create a profile and add Device Guard from the settings picker. After selecting Device Guard, select Credential Guard from the policy settings.

## Associated Techniques

-	[T1003.001](https://attack.mitre.org/techniques/T1003/001/) – OS Credential Dumping: LSASS Memory
-	[T1003.004](https://attack.mitre.org/techniques/T1003/004/) – OS Credential Dumping: LSA Secrets
-	[T1555.004](https://attack.mitre.org/techniques/T1555/004/) – Credentials from Password Stores: Windows Credential Manager
-	[T1550.002](https://attack.mitre.org/techniques/T1550/002/) – Use Alternate Authentication Material: Pass the Hash
-	[T1550.003](https://attack.mitre.org/techniques/T1550/003/) – Use Alternate Authentication Material: Pass the Ticket

## Related Countermeasures

- CM0004 - Enable Windows Defender Exploit Guard (WDEG) Attack Surface Reduction (ASR) Rules
- CM0079 - Reset Active Directory Services (AD DS) Connector Account Password
- CM0087 - Disable Credential Caching in the WDigest Authentication Protocol
- CM0089 - Enable Local Security Authority (LSA) Protections
- CM0090 - Detect Attempts to Access Local Security Authority Subsystem Service (LSASS) Process
- CM0094 - Restrict Domain Admin User Rights and Monitor Protected Users on Workstations
- CM0096 - Prevent and Detect Attempts to Copy NT Directory Services Database Information Table (NTDS.dit)
- CM0104 - Enable Interactive Logon and Require Smart Card Policy for Privileged User Accounts

## References

- Credential Guard overview | <https://learn.microsoft.com/en-us/windows/security/identity-protection/credential-guard>
- Configure Credential Guard | <https://learn.microsoft.com/en-us/windows/security/identity-protection/credential-guard/configure?tabs=intune>
- Considerations and known issues when using Credential Guard | <https://learn.microsoft.com/en-us/windows/security/identity-protection/credential-guard/considerations-known-issues>
- How Credential Guard works | <https://learn.microsoft.com/en-us/windows/security/identity-protection/credential-guard/how-it-works>
- Virtualization-based security must be enabled with the platform security level configured to Secure Boot or Secure Boot with DMA Protection. | <https://www.stigviewer.com/stig/windows_server_2016/2018-03-07/finding/V-73513>
