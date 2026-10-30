# Configure and Invalidate Relative ID (RID) Pools

* **ID:** CM0136
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Examine, Disable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; RID pool configuration and invalidation

## Intended Outcome

Configuring and invalidating relative ID (RID) pools blocks or restricts an adversary's attempts to escalate privileges and achieve persistence by hijacking RID.  

## Introduction

A relative ID (RID) is a unique number assigned to an object in a domain, which is essential to distinguishing between different objects (such as user accounts or groups) within the same domain. Errors or misconfigurations of RID can cause misauthentication or integrity problems in the environment, such as errors during group or user creation, inability to move objects, or users being unable to authenticate.

Adversaries can change an account's RID to grant themselves administrative permissions by modifying the registry. Through this method, an adversary can maintain higher privileges and persistence in an environment using valid accounts. Changing a Guest account's RID (500) to an Administrator account's RID (501) is a common attack against RID, and grants adversaries escalated privileges.     

## Preparation

- Identify all domain controllers in the environment. 

## Risks

- After invalidating RID pools, subsequent attempts to create new objects or security principals may initially fail. Retry the operation: the second time should succeed due to reallocation of RID pools.  

## Guidance

### Invalidate current RID pools

Administrator privileges are required to invalidate RID pools via PowerShell. 

- On each domain controller, use PowerShell to run the following script:
	```
	$Domain = New-Object System.DirectoryServices.DirectoryEntry
	$DomainSid = $Domain.objectSid
	$RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")
	$RootDSE.UsePropertyCache = $false
	$RootDSE.Put("invalidateRidPool", $DomainSid.Value)
	$RootDSE.SetInfo()
	```
- Check event logs for event ID 16654 to ensure the command was successful. 

### Configure RID pools

- Raise the value of available RID pools via adsedit and a calculator:
	- In the Server Manager, navigate to `Tools > ADSI Edit > Connect to` and select the Default Naming Context. 
	- Navigate to `CN=RID Manager$,CN=System,DC=<domain name>`. 
	- Select properties of `CN=RID Manager$`, select edit on the `rIDAvailablePool` attribute and copy the integer value into your clipboard. 
	- Add desired value to the copied integer (make sure to use scientific mode). 
	- Copy the new value back into the `rIDAvailablePool` attribute, click `Save` then click `Apply`. 

- Raise the value of available RID pools via LDP: 
	- In the command prompt, type `ldp` and hit Enter. In the dialog that pops up, navigate to `Connection > Connect type` and type the name of the RID manager.
	- Select `Connection > Bind > Bind with credentials` and enter administrative credentials. 
	- Navigate to `View > Tree > CN=RID Manager$,CN=System,DC=domain name > Browse > Modify`, and add the desired value to the current `rIDAvailablePool` value. Type the sum into the `Values` field. 
	- In the DN field, enter `cn=RID Manager$,cn=System,dc=<domain name>` and in the Edit Entry Attribute field, enter `rIDAvailablePool`.
	- Select `Run`, then `Close`. 

## Associated Techniques

- [T1098](https://attack.mitre.org/techniques/T1098/) - Account Manipulation
- [T1078](https://attack.mitre.org/techniques/T1078/) - Valid Accounts
- [T1548](https://attack.mitre.org/techniques/T1548/) - Abuse Elevation Control Mechanism (?)

## Related Countermeasures

- CM0064 - Investigate Account Manipulation
- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts
- CM0044 - Monitor Account Creation and Permission Changes
- CM0114 - Enable Microsoft Conditional Access Policy Templates
- CM0146 - Configure Flexible Single Master Operations (FSMO) Roles

## References

- Security identifiers | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-identifiers#relative-identifier-allocation>
- Active Directory Forest Recovery - Invalidate the current RID pool | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/forest-recovery-guide/ad-forest-recovery-invaildate-rid-pool>
- Active Directory Forest Recovery - Raise the value of available RID pools | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/forest-recovery-guide/ad-forest-recovery-raise-rid-pool>
- Persistence – RID Hijacking | <https://pentestlab.blog/2020/02/12/persistence-rid-hijacking/>