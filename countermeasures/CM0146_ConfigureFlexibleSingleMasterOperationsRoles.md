# Configure Flexible Single Master Operations (FSMO) Roles

* **ID:** CM0146
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; FSMO role transfer and domain controller recovery

## Intended Outcome

Configuring flexible single master operations (FSMO) roles slows down an adversary's exploitation of the domain via compromised domain controllers. 

## Introduction

Flexible single master operations (FSMO) roles are assigned to domain controllers (DCs) and perform domain and forest management tasks to keep a directory functioning properly. Instead of one DC performing every task in a domain, they got split up into 5 roles at the domain and forest levels. Domain controllers hosting these roles should always be available, and operation masters should be in areas with reliable networks. 

While it is possible to configure all roles onto the same domain controller, if an organization has multiple controllers or forests, splitting up the FSMO roles may make it more difficult for attackers to compromise all the domain functionality from one controller. They would need to compromise several DCs to achieve full control, giving defenders more time to notice the compromise and take action. Post-incident, splitting up the roles may remove some FSMO privileges from attackers who had gained access to a DC. Reconfiguring roles helps recover domain functionality if any DCs have been taken offline or compromised and quarantined. 

## Preparation

- Identify domain controllers suitable for FMSO roles. 
- Ensure process logging is active in the environment. 

## Risks

- Configuring roles on unavailable domain controllers or controllers that may go down will cause the DC to be unable to perform that role. This may be detrimental to the domain and forest, and therefore the organization. Ensure all DCs with FSMO roles are available and meet network reliability requirements. 

## Guidance

Under normal conditions, all five FSMO roles must be assigned to a domain controller in the environment. 

### Placement of roles

- Configure a separate domain controller as a stand-by operations master for forest roles.
- Configure a separate domain controller as a stand-by operations master for domain roles. 
- Place domain-level roles on a high performance domain controller.
- Place forest-level roles on a domain controller in the root of the forest. 

Roles can be transferred via the Ntdsutil.exe or Microsoft Management Console (MMC) snap-ins. Ensure the machine has AD RSAT tool installed or is a domain controller in the forest where roles are being transferred. 

In some situations roles have to be seized instead of being transferred, for example if the domain controller possessing the role is no longer operational or gets demoted.

### Transferring or Seizing Roles via Ntdsutil

- Run the ntdsutil utility, type `roles` and hit Enter.
- Type `connections` and hit Enter.
- Type `connect to server <servername>`, where `<servername>` is the name of the domain controller to receive the roles, and hit Enter. 
- To transfer the role, type `transfer <role>`.
- To seize the role, type `seize <role>`. 

### Monitoring for FSMO role enumeration techniques

- Monitor for the `netdom query fsmo` command.
- Monitor for the `Get-ADForest <domain-name> | Format-Table SchemaMaster,DomainNamingMaster` and `Get-ADDomain <domain-name> | format-table ` cmdlets or command lines. 

## Associated Techniques

- [T1569](https://attack.mitre.org/techniques/T1569) - System Services

## Related Countermeasures

- CM0136 - Configure and Invalidate Relative ID (RID) Pools
- CM0145 - Reset Interdomain Trust Account (ITA) Credentials

## References

- Flexible Single Master Operations roles in Active Directory Domain Services | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-fsmo-roles>
- Active Directory FSMO roles in Windows | <https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/fsmo-roles>
- Transfer or seize Operation Master roles in Active Directory Domain Services | <https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/transfer-or-seize-operation-master-roles-in-ad-ds>
