# Reset User Account Passwords

* **ID:** CM0033
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Identity administration; user accounts and password resets

## Intended Outcome

Resetting user account passwords restricts adversary persistence and lateral movement using valid accounts.

## Introduction

No introduction content identified.

## Preparation

- Configure environment to support self-service password reset (SSPR) for users.
- Develop standard operating procedures (SOPs) when pushing out mandatory password resets to affected accounts. Instructions should include guidance for alerting users and instructions on self-service password resets.
- Ensure a means of attestation is defined to verify the veracity of users requiring a manual password reset.


## Risks

- This countermeasure may have impacts that disrupt operations.
- Organization's helpdesk may be inundated with assistance reset if a password reset is pushed. Staging the password reset will mitigate the burden on both users and service desk technicians. 
- Reducing the password expiration age or gradually resetting passwords for user accounts in timed batches, while often necessary, will risk the adversary maintaining authenticated sessions until the hijacked account's password is reset.
- Remote users may be unable to fulfill their job responsibilities until they reset their password and successfully authenticate on the domain. 
- Adversaries that manage to exfiltrate the Active Directory Directory Services (AD DS) database or are abusing the SSPR may be able to re-establish initial access or persistence via SSPR.  

## Guidance

- Post-breach, configure domains to request password resets for user accounts. 
- For onsite users with direct access to domain controllers, password resets can be performed in batches by resetting passwords by organizational units and leveraging the "User must change password at next logon” PowerShell flag. 
- Alternately, Fine Grained Password Policies (FGPP) and reducing password age through domain policy modifications may facilitate mass password resets of user accounts.
- For hybrid organizations using Microsoft Entra ID, administrators may use Microsoft Graph to set user attributes to either "forceChangePasswordNextSignIn" or "forceChangePasswordNextSignInWithMfa." 
- Ensure compliance with best practices, to include:
    - Salting passwords
    - Increasing password length and complexity
    - Prevent password reuse 

    

## Associated Techniques

- [T1110](https://attack.mitre.org/techniques/T1110) - Brute Force
- [T1555](https://attack.mitre.org/techniques/T1555) - Credentials from Password Stores

## Related Countermeasures

- CM0018 - Enable Active Directory (AD) Protected Users Security Group
- RM0138 - Reset Passwords for Cloud Accounts (*future*)
- RM0140 - Reset Passwords for Root Accounts (*future*)
- RM0326 - Reset Passwords for AD Accounts with Elevated Privileges (*future*)
- CM0028 - Reset Service Account Passwords
- CM0068 - Audit and Restrict Exchange ApplicationImpersonation Role
- RM0330 - Reset Passwords for Accounts with Compromised Credentials (*future*)
- CM0029 - Reset NT Hashes for Smart Card-enabled Accounts
- CM0069 - Reset Directory Services Restore Mode (DSRM) Account Passwords on Domain Controllers
- RM0335 - Reset Passwords for non-AD High-Value Asset User Accounts (*future*)
- RM0336 - Reset Password for Network Device Administrative Accounts (*future*)
- CM0071 - Unenroll Suspicious Multifactor Authentication (MFA) Device from Microsoft User Account
- CM0075 - Implement Access Restrictions on Cloud Storage Objects
- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts
- CM0112 - Remove Extraneous and Stale Accounts
- CM0118 - Revoke Existing Administrator Permissions and Deploy New Administrator Accounts
- CM0121 - Enable Managed Domain Authentication
- CM0124 - Disable On-Premise Active Directory (AD) Accounts with Privileged Roles in Entra ID
- CM0125 - Reset and Monitor adminCount Attribute

## References

- Digital Identity Guidelines | <https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-63b.pdf>
- Set the password expiration policy for your organization | <https://learn.microsoft.com/en-us/microsoft-365/admin/manage/set-password-expiration-policy>
