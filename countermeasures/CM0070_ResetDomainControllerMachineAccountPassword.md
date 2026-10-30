# Reset Domain Controller (DC) Machine Account Password

* **ID:** CM0070
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Active Directory administration; domain controller machine accounts

## Intended Outcome

Resetting the Domain Controller's account password terminates adversary persistence using stolen domain controller machine account credentials. 

## Introduction

Every Windows-based computer has a machine account (also known as the Active Directory (AD) Computer Object) password. These passwords are used to authenticate onto a domain and are typically managed by the Netlogon service, which resets expired passwords and maintains a machine's connection to the domain.

## Preparation

- Resetting a domain controller's (DC) machine account password requires domain administrator rights on both the machine and the computer object in AD. It may be appropriate to reset all domain administrator passwords prior to resetting the machine account password for DCs. 

## Risks

- Resetting the password for the DC account may risk breaking synchronization of the DC if done improperly, such as by using the `Reset-ComputerMachinePassword` cmdlet.

## Guidance

### Reset DC Machine Account Password with Netdom.exe

1. Login to the DC whose password is to be changed. 

2. Stop and disable the Kerberos Key Distribution Center (KDC) service on the DC and select Manual for startup type. 

3. Clear Kerberos ticket cache on the DC with KLIST (command is `klist purge`), Kerbtest or KerbTray tools.

4. Reset the domain controller's machine password using the command `netdom resetpwd /s:<server> /ud:<domain\User> /pd:*`. Run this command twice to reset the current password and overwrite the previous password for the account.

Replace `<server>` with the domain controller and `<domain/User>` with a user account on the domain with administrator privileges in the `domain\User format. 

System administrators can run `netdom help resetpwd` to verify appropriate syntax. 

5. Restart the domain controller.

6. Re-enable the KDC on the DC and return startup type to Automatic.

7. Verify that the password for the DC machine account has changed by checking the pwdLastSet attribute.

8. Repeat the process on all other DCs. The command `repadmin /syncall /AdeP` can be used to force changes to replicate across DCs.

## Associated Techniques

- [T1078.002](https://attack.mitre.org/techniques/T1078/002/) - Valid Accounts: Domain Accounts  

## Related Countermeasures

-   CM0050 - Reset Kerberos Ticket Granting Ticket (KRBTGT) Password Twice
-   CM0028 - Reset Service Account Passwords
-   CM0079 - Reset Active Directory Services (AD DS) Connector Account Password

## References

- Active Directory Forest Recovery - Reset the computer account on the DC legacy | <https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/forest-recovery-guide/ad-forest-recovery-reset-computer-account-dc>
- Practical Compromise Recovery Guidance for Active Directory | <https://m365internals.com/2021/04/27/practical-compromise-recovery-guidance-for-active-directory/>
- Use Netdom.exe to reset machine account passwords of a Windows Server domain controller | <https://learn.microsoft.com/en-us/troubleshoot/windows-server/windows-security/use-netdom-reset-domain-controller-password>
