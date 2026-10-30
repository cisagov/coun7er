# Rebuild Group Managed Service Accounts (gMSA)

* **ID:** CM0135
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; gMSA rebuilding and Key Distribution Service root keys

## Intended Outcome

Rebuilding group managed service accounts (gMSA) prevents an adversary's attempts at initial compromise and privilege escalation by abusing gMSA. 

## Introduction

Group Managed Service Accounts (gMSA) are accounts used by services and often require elevated privileges in an environment. They are managed by the Active Directory (AD) itself, rather than by users. Passwords for gMSA are randomly generated every 30 days (by default; may be configured) by the key distribution service (KDS) from a set root key within the AD.

Adversaries can compromise managed accounts with pass-the-hash attacks or other exploits targeting the Lightweight Directory Access Protocol (LDAP) and use gMSA permissions to perform privileged actions. Resetting the password on the account is not always an effective mitigation, as attackers can launch a "Golden gMSA Attack" and calculate future as well as current gMSA passwords, as long as they have seen the root key once. To prevent this, rebuilding the accounts completely and generating a new root key is recommended.      

## Preparation

- Identify which machines need service accounts.
- Identify which services need accounts. 
- Ensure the Active Directory PowerShell module is installed on all machines that the service account needs access to. 
- Ensure process logging is enabled in the environment. 

## Risks

- Service accounts will be unavailable in the window of time between disabling them and rebuilding; consider performing the operation outside of active business hours or during slower hours. 
- After creating a root key, a domain controller will need at least 10 hours to allow all other domain controllers to synchronize replication before new gMSA can be created.
- Rebuilding the root key may cause issues where the old key continues to be used after deletion due to caching. Consider restarting the Key Distribution Service (KDS) on all domain controllers after recreation.

## Guidance

### Remove old gMSA

- Identify all gMSA with the `Get-ADServiceAccount -Filter *` command.
- Remove gMSA accounts with the `Remove-ADServiceAccount` cmdlet. 
- Remove old KDS root key. 

### Build new gMSA

- Create new KDS root key with the `Add-KdsRootKey -EffectiveImmediately` cmdlet. 
- Restart all domain controllers to avoid caching and replication issues. 
- Create new security group for the accounts.
- Add all users and computers the managed accounts should have access to into the group. Computers may need to be rebooted before this change is applied. 
- Create accounts with the `New-ADServiceAccount` cmdlet; specify the newly created security group for password retrieval.
- Install the account with the `Install-ADServiceAccount` cmdlet.
- Optionally, give the accounts a Service Principal Name with the `setspn -a <SPN> <USER>` command; this will allow the account to authenticate using Kerberos. 

### Monitor for gMSA enumeration techniques

- Monitor for process calls to BloodHound or Mimikatz.
- Monitor for the `Get-ADServiceAccount -Filter * -Properties * ` command.
- Monitor for suspicious PSExec or LDAP activity in the environment. 
- Monitor for access to LSA secrets or modification to the `HKLM\SECURITY\Policy\Secrets` registry key. 

## Associated Techniques

- [T1078](https://attack.mitre.org/techniques/T1078/) - Valid Accounts
- [T1555](https://attack.mitre.org/techniques/T1555/) - Credentials from Password Stores
- [T1003.004](https://attack.mitre.org/techniques/T1003/004/) - OS Credential Dumping: LSA Secrets 

## Related Countermeasures

- CM0050 - Reset Kerberos Ticket Granting Ticket (KRBTGT) Password Twice
- CM0105 - Remove Malicious Enterprise Applications and Service Account Principals
- CM0023 - Identify and Terminate Suspicious Processes
- CM0028 - Reset Service Account Passwords

## References

- Attacking Group Managed Service Accounts (gMSA) | <https://medium.com/@offsecdeer/attacking-group-managed-service-accounts-gmsa-5e9c54c56e49>
- Secure group managed service accounts | <https://learn.microsoft.com/en-us/entra/architecture/service-accounts-group-managed>
- Create the Key Distribution Services KDS Root Key | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-managed-service-accounts/group-managed-service-accounts/create-the-key-distribution-services-kds-root-key>