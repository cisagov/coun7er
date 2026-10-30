# Enable Multiple Administrative Approval (MAA) for Access Policies

* **ID:** CM0111
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Microsoft Intune administration; access policies and multiple administrative approval

## Intended Outcome

Enabling multiple administrative approval (MAA) across Microsoft access policies restricts an adversary from compromising a domain/administrative account to achieve privilege escalation and persistence.

## Introduction

It is important to apply secure access policies to standalone or active directory Microsoft accounts. Multiple administrative approval (MAA) is a best practice relevant to Microsoft Intune that requires a second administrative account to be used in approving a configuration change against a specific account. When MAA is properly implemented, a configuration change to a tenant resource that is protected by MAA won't be applied until a different account explicitly approves the change. In the context of Intune tenants, access policies support resources such as apps, scripts, and other MAA policies.

## Preparation

- Take inventory of all account types and administrative accounts that exist within the environment/tenant.
- Ensure that at least two administrator accounts are present in order to allow operation of MAA access policies.
- Accounts used in the creation of access policies must be assigned the appropriate "Intune Service Administrator" or "Azure Global Administrator" role.
- Ensure that the account(s) that will approve access policies are delegated to the approver group assigned to the access policy for a specific resource (app, script, or access policy).

## Risks

- Modification of access policies to implement MAA may lead to misconfigured resources or improperly defined access policies.
- Introducing MAA to access policies requires approving entities for any attempted changes against resources or other access policies. This introduces delay to the processing of configuration changes.
- Improper delegation of admin accounts to the approval group/role may cause unwarranted changes such as unauthorized script or app deployments.

## Guidance

### Create an MAA Access Policy

*Per Microsoft Documentation*

1. To create an access policy, in the Microsoft Intune admin center (<https://go.microsoft.com/fwlink/?linkid=2109431>), go to **Tenant administration** -> **Multi Admin Administration** -> **Access policies** and select **Create**.
    
2. On the _Basics_ page, provide a _Name_, and optional _Description_, and for _Profile type_ select from available options. Each policy supports a single profile type.
    
3. On the _Approvers page_, select **Add groups** and then select a group as the group of approvers for this policy. More complex configurations that exclude groups aren't supported.
    
4. On the _Review + Create_ page, review, and then save your changes. After Intune applies this policy, configurations for the protected profile type will require multiple admin approvals.

- When creating and implementing an MAA access policy, it is important to define clear approval processes to minimize delays in the processing of configuration changes. Establishment of Role-Based Access Control (RBAC) is recommended to mitigate the potential for damage caused by improper delegation of admin accounts to the approval group.

### Submit a Resource Configuration Request under MAA

- Once MAA is enabled, requests must be submitted and approved before a resource can be created or edited.

- When submitting a request, standard procedure for creation/editing of a resource within Microsoft Intune should be followed, up until the final page before changes are saved.

- Add details to the *Business justification* field before submitting a request.

- The status of pending requests can be monitored in the Microsoft Intune admin center by going to **Tenant administration > Multi Admin Approval > My requests**.

- Requests will remain here, pending approval by anyone within the known list of approvers. Requests can be cancelled before approval by selecting it from **My requests > click Cancel request**.

### Approve a Resource Configuration Request under MAA

1. To find requests to approve, in the Microsoft Intune admin center (<https://go.microsoft.com/fwlink/?linkid=2109431>) go to **Tenant administration** > **Multi Admin Administration** > **Received requests**.
    
2. Select the _Business justification_ link for a request to open the review page where you can learn more about the request, and manage approval or rejection.
    
3. After reviewing the details, enter relevant details in the Approver notes field, and then select **Approve request** or **Reject request**.
    
4. After you approve a request, the requestor needs select **Complete**. Intune will process the change, and changes the status to _Completed._ Verify the approval succeeded (or failed) by reviewing the console notification upon completion.
    
    To verify if the approval succeeded (or failed), look at the notifications in the Intune admin center. A message shows if the approval succeeded or failed.

## Associated Techniques

- [T1078.002](https://attack.mitre.org/techniques/T1078/002/) - Valid Accounts: Domain Accounts
- [T1098](https://attack.mitre.org/techniques/T1098/) - Account Manipulation

## Related Countermeasures

- CM0084 - Restrict Accounts with Privileged Active Directory (AD) Access from Logging into Endpoints
- CM0107 - Restore SYSVOL Content in AD Domains
- CM0114 - Enable Microsoft Conditional Access Policy Templates
- CM0118 - Revoke Existing Administrator Permissions and Deploy New Administrator Accounts
- CM0121 - Enable Managed Domain Authentication
- CM0142 - Enable LDAP Signing and Channel Binding

## References

- Use Access Policies to require multiple administrative approvals | <https://learn.microsoft.com/en-us/mem/intune/fundamentals/multi-admin-approval>

- Avenues to Compromise - Microsoft | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/avenues-to-compromise>

- Microsoft Intune securely manages identities, manages apps, and manages devices | <https://learn.microsoft.com/en-us/mem/intune/fundamentals/what-is-intune>
