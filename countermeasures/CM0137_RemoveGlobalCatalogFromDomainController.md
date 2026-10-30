# Remove Global Catalog from Domain Controller

* **ID:** CM0137
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Eliminate
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; Global Catalog roles and domain controllers, process monitoring

## Intended Outcome

Removing the global catalog from domain controllers prevents an adversary's attempts at discovery via the global catalog. 

## Introduction

The global catalog is a lightweight directory access protocol (LDAP) compliant server in a domain that allows users and applications to find objects within the domain tree. A global catalog contains partial replicas of all naming contexts within the domain, including schema and configuration naming contexts. Users can use the global catalog to quickly locate correct objects based on the most frequently-used attributes. LDAP traffic on ports 3268 and 3269 query the global catalog. 

When recovering or restoring a domain controller or active directory, the recovered catalog may hold newer information about a domain that no longer exists; the newer data may replicate throughout the rest of the domains. Therefore the global catalog must be removed from the environment, as that removes the partial replicas as well. 

Adversaries can abuse the global catalog for domain enumeration, aiding in their discovery of the environment. 

## Preparation

- Identify which domain controller(s) have a global catalog. 

## Risks

- Domain access may take longer since requests would have to resolve via the root domain controller instead of a partial replica as stored in the global catalog. 

## Guidance

- In the Active Directory server manager (or the Microsoft Entra admin center?) navigate to the `Active Directory Sites and Services`. 
- Select appropriate target server and navigate to `NTDS Settings > Properties`.
- Deselect the `Global Catalog` checkbox and select apply. 

- If Repadmin is present and used in the environment, the following command can be used to remove the global catalog: `repadmin.exe /options DC_NAME –IS_GC`.

### Monitor for enumeration techniques via the global catalog

- Monitor process calls to BloodHound.
	- The SharpHound component looks like SMB scanning techniques over ports 137 and 445; keep an eye on all instances of SMB scans. This may cause a lot of noise in larger organizations. 
	- Monitor commands including `-collectionMethod`,  `invoke-bloodhound`, `azurehound` or `get-bloodHounddata` in the command line. 
- Monitor use of other Active Directory discovery and enumeration tools such as AD Hunt, PowerView, or Adalanche.
	- Events with ID 4799 may show enumeration attempts on local group membership. 
    - Monitor for process creation of processes with names containing `adhunt.py` or `adalanche`.
    - Monitor for command lines containing PowerView modules, a detailed list of which is provided at <https://powersploit.readthedocs.io/en/latest/Recon/>. 
- Check for event ID 5156(S), or the Windows Filtering Platform allowing a connection. Check if the origins of the connection are from the system directory or other expected location. This may be very noisy and cause many false positives. 

## Associated Techniques

- [T1590.001](https://attack.mitre.org/techniques/T1590/001/) - Gather Victim Network Information: Domain Properties
- [T1087.002](https://attack.mitre.org/techniques/T1087/002/) - Account Discovery: Domain Account

## Related Countermeasures

- CM0063 - Investigate Suspicious Login Attempts
- CM0065 - Isolate Endpoints from Network
- CM0030 - Remove Known Malware

## References

- Global Catalog | <https://learn.microsoft.com/en-us/windows/win32/ad/global-catalog>
- Active Directory Forest Recovery - Remove the global catalog | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/forest-recovery-guide/ad-forest-recovery-remove-gc>
- BloodHound | <https://redcanary.com/threat-detection-report/threats/bloodhound/>
- 5156(S): The Windows Filtering Platform has permitted a connection. | <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-5156>
- ADHunt | <https://github.com/asmtlab/ADHunt>
- Adalanche | <https://github.com/lkarlslund/Adalanche>
