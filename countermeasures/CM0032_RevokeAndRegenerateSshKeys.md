# Revoke and Regenerate SSH Keys

* **ID:** CM0032
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Systems administration; SSH keys and secure remote access

## Intended Outcome

Revoking and regenerating SSH keys terminates adversary persistence and lateral movement using compromised private keys.

## Introduction

No Introduction content identified.

## Preparation

- Administrators should inventory all systems and devices that use SSH keys to be prepared in the event of an incident.

## Risks

- Errors can render this countermeasure ineffective.
- Regenerating SSH keys on a compromised host (e.g. jump box, bastion host) will allow adversaries to access and use the new keys. Eliminate the adversary's access to the host before regenerating the SSH keys.
- This countermeasure may have impacts that disrupt operations.
- Activities relying on the SSH service will be disrupted during the key revocation, regeneration and server restart process.

## Guidance

- Revoke SSH Keys
    - Access remote hosts and revoke public keys associated with the compromised private key. For a small number of systems, this can be performed by manually updating the authorized_keys file. 
- Regenerate SSH Keys
    - After revoking public keys on all servers where the original system was granted access, system administrators should replace the compromised private key using OpenSSH or a Centralized Key Management System.  If key generation is required at a larger scale, consider automating the process using frameworks like Ansible or Puppet.    
- Update the Public Keys on All Servers
- Restart SSH service
    - By restarting the SSH service, users using the revoked key pair will be booted off remote systems and attempts to reinitialize using the blacklisted key will fail.
- Restrict SSH
    - In addition to revoking and generating new SSH keys, system administrators should consider implementing additional restrictions on SSH use and access.
- Disable SSH Daemon
    - In some cases, system administrators may find it necessary to disable the SSH Daemon.
- Restrict Access to SSH in UNIX
- Restrict Access to SSH Daemon
- Restrict Access to the Authorized_Keys File
- Disable Root Login Over SSH
    - Disable root login over SSH to reduce the ability of adversaries to brute-force systems remotely.

## Associated Techniques

- [T1098.004](https://attack.mitre.org/techniques/T1098/004) - SSH Authorized Keys
- [T1552.004](https://attack.mitre.org/techniques/T1552/004) - Unsecured Credentials: Private Keys

## Related Countermeasures

- CM0013 - Block Windows Management Instrumentation (WMI) Service
- CM0020 - Disable Virtual Network Computing (VNC) Desktop-sharing System
- CM0076 - Remove Adversary Certificates and Rotate Secrets for Applications and Service Principals
- CM0036 - Identify and Monitor Remote Access Tools

## References

- Key-based authentication in OpenSSH for Windows | <https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement>
- Authorized Key in SSH | <https://www.ssh.com/academy/ssh/authorized-key>
- NISTIR 7966: Security of Interactive and Automated Access Management Using Secure Shell (SSH) | <https://nvlpubs.nist.gov/nistpubs/ir/2015/nist.ir.7966.pdf>
- Using PuTTYgen on Windows to generate SSH key pairs | <https://www.ssh.com/academy/ssh/putty/windows/puttygen>
- How (and Why) to Disable Root Login Over SSH on Linux | <https://www.howtogeek.com/828538/how-and-why-to-disable-root-login-over-ssh-on-linux/>
- Configure SSH to use two-factor authentication | <https://ubuntu.com/tutorials/configure-ssh-2fa#3-configuring-authentication>
- OpenSSH/Cookbook/Certificate-based Authentication | <https://en.wikibooks.org/wiki/OpenSSH/Cookbook/Certificate-based_Authentication>
