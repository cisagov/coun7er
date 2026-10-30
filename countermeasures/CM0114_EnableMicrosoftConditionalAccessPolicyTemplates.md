# Enable Microsoft Conditional Access Policy Templates

* **ID:** CM0114
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Microsoft Entra ID administration; Conditional Access templates and access controls

## Intended Outcome

Enabling Microsoft Conditional Access Policy templates blocks or restricts an adversary from lateral movement and privilege escalation using legitimate credentials. 

## Introduction

Microsoft Conditional Access is a zero-trust security parameter to help manage user and device identity, taking signals from multiple sources; if users want to access resources, they have to perform some action (the condition). Administrators can use conditional access policies to implement access controls for their organization.

Commonly applied policies include requiring multifactor authentication (MFA) for users with administrator roles and privileges, requiring MFA for Microsoft Azure management, blocking users and accounts using legacy authentication protocols, and blocking access attempts from specific geographic locations. Multiple policies may apply to a single user, and in that case both policies must be fulfilled before access is granted. Conditional access makes it harder for adversaries to use legitimate credentials to access a network remotely, since policies may require MFA or a trusted device before granting access.

## Preparation

- Add and configure appropriate licenses. Conditional Access Policies require the Microsoft Entra ID P1 or Microsoft 365 Business Premium license. Any risk-based policies require an Entra ID P2 license and access to Microsoft Entra ID Protection.  
- Identify appropriate access policies for users, devices, services, and roles.
- Identify which templates or policies within the templates fit organizational and business needs. 

## Risks

Changing access control policies and methods can be disruptive to operations, as users may lose access to their accounts or need to set up additional authentication methods. 


## Guidance

To minimize disruption caused by changing access control, consider rolling out policies in phases so help desk support is not overwhelmed and alerting users of changes.

Microsoft provides templates in the following categories:
- Secure Foundation
- Zero Trust
- Remote Work
- Protect Administrator
- Emerging Threats

Some policies may require additional licenses. 

Navigate to the templates in the Microsoft Entra admin center under `Protection > Conditional Access > Create new policy from templates`.
- Select the appropriate policies and templates. 
- Add users and groups the policies will apply to. The creators of the policies will be excluded; to exclude other users, modify the policy after creation. Delete the account used to create the policy if exclusions are not required.  

### Develop New Policies for Future Incidents (Optional)

Microsoft allows policy creation without using templates. Configure conditional access policies aligning with organizational business needs to aid in future incident response. 
- In Microsoft Entra admin center, navigate to `Protection > Conditional Access`.
- Select `Create new policy`, and enter the name for the policy.
- Set Assignments to `Users or workload identities` and ensure that under `What does this policy apply to?` and `Include`, `Users and groups` is selected.
- Add users or groups to the policy.
- Configure conditions for the policy. 

For more detailed walkthrough steps, see "Tutorial: Secure user sign-in events with Microsoft Entra multifactor authentication" in the **References** section. 

## Associated Techniques

- [T1550](https://attack.mitre.org/techniques/T1550) - Use Alternate Authentication Material
- [T1078](https://attack.mitre.org/techniques/T1078) - Valid Accounts

## Related Countermeasures

- CM0003 - Configure File and Directory Permissions
- CM0063 - Investigate Suspicious Login Attempts
- CM0084 - Restrict Accounts with Privileged Active Directory (AD) Access from Logging into Endpoints
- CM0037 - Enable Geolocation-based Traffic Filtering
- CM0043 - Monitor Permissions for User, Service, and Administrator Accounts
- CM0044 - Monitor Account Creation and Permission Changes
- CM0111 - Enable Multiple Administrative Approval (MAA) for Access Policies
- CM0115 - Block Untrusted and Unsigned Processes that Run from USB
- CM0136 - Configure and Invalidate Relative ID (RID) Pools
- CM0139 - Rebuild Active Directory (AD) Connect Server

## References

- What is Conditional Access? | <https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview>
- Conditional Access policy templates | <https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-conditional-access-policy-common?tabs=secure-foundation>
- Configuring Azure Active Directory Conditional Access | <https://learn.microsoft.com/en-us/appcenter/general/configuring-aad-conditional-access>
- Tutorial: Secure user sign-in events with Microsoft Entra multifactor authentication | <https://learn.microsoft.com/en-us/entra/identity/authentication/tutorial-enable-azure-mfa?bc=%2Fazure%2Factive-directory%2Fconditional-access%2Fbreadcrumb%2Ftoc.json&toc=%2Fazure%2Factive-directory%2Fconditional-access%2Ftoc.json#create-a-conditional-access-policy>


