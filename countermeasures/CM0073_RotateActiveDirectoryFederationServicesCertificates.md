# Rotate Active Directory Federation Services (AD FS) Certificates

* **ID:** CM0073
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** ADFS administration; certificates and federation services

## Intended Outcome

Rotating Active Directory Federation Services (AD FS) certificates terminates adversary credential access via forged Security Assigned Markup Language (SAML) tokens.

## Introduction

AD FS certificates are used to decrypt and digitally sign all tokens that AD FS produces, ensuring that the tokens are tamper-proof and originate from a trusted source. These certificates typically have a default lifespan of one year.

An adversary that compromises either the AD FS service account or an account with local administrative permissions on AD FS servers may be able to extract the token-signing certificate. An adversary that uses the certificate to create forged SAML tokens or generates their own token-signing certificate will be able to authenticate across services that use SAML 2.0 as a single sign-on (SSO) mechanism.

## Preparation

- Document the on-premises and cloud Token Signing Certificate thumbprint and expiration dates. This can be accomplished after connecting to the Microsoft Online Service with `Connect-MsolService` and running `Get-MsolFederationProperty -DomainName <domain>` or by using AD FS Management under Service > Certificates.
- Determine whether  `AutoCertificateRollover` is set to `True` before generating a new self-signed certificate. This can be done using the PowerShell command `Get-AdfsProperties | FL AutoCert*, Certificate*` If `AutoCertificateRollover` is set to false, a new certificate will have to be generated manually.
- Consult Microsoft's Emergency rotation of the AD FS certificates from Microsoft Entra documentation before proceeding with rotation of the AD FS certificates. Available at <https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-emergency-ad-fs-certificate-rotation#remove-your-old-certificates>

## Risks

- This countermeasure may disrupt operations. For instance, rotating AD FS certificates may cause a service outage as trusts update to use the new certificates. Outages typically resolve after all the federation partners have the new certificates.
- Manually importing a certificate from an untrusted source or compromised certificate authority may risk undermining efforts to contain the adversary

## Guidance

### Generate AD FS Certificates

#### Automatically Generate Self-signed AD FS Certificate with PowerShell

To automatically generate a new token signing certificate run the following PowerShell command once logged into the primary AD FS server. The `-Urgent` flag will replace the primary certificate immediately without making a Secondary certificate.

`Update-ADFSCertificate -CertificateType Token-Signing -Urgent`
`Update-ADFSCertificate -CertificateType Token-Decrypting -Urgent`


To generate a second token certificate:

`Update-ADFSCertificate -CertificateType Token-Signing`
`Update-ADFSCertificate -CertificateType Token-Decrypting`

To verify that a new token certificates are generated, the following PowerShell command may be run:

`Get-ADFSCertificate -CertificateType Token-Signing`
`Get-ADFSCertificate -CertificateType Token-Decrypting`

#### Manually Generate New AD FS Certificate

AD FS servers where `AutoCertificateRollover` is set to `false` must acquire two new certificates from their certificate authority and import them into the local machine personal certificate store on each federation server. Certificates must be obtained from trusted sources. Backup certificates stored on the local AD FS servers should not be used.

Configuring a new primary and secondary certificates can be accomplished with the AD FS management console. 

### Update Entra ID with New Signing Certificates

Run Azure AD PowerShell, connect to Microsoft Entra ID, and sign in with hybrid identity credentials on the primary federation server. To update the certificate information, administrators should run the PowerShell command `Update-MsolFederatedDomain` and provide the domain name when prompted. The flag `-SupportMultipleDomain` may be necessary if an error is received. 

### Replace SSL Certificates

After rotating AD FS token signing certificates, organizations should revoke and replace their SSL certificate for AD FS and their Web Application Proxy (WAP) server. 

- When a new certificate is provided from a trusted certificate authority, update the primary  AD FS server with the PowerShell command `Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'` to rotate the SSL certificate. 
- Run `Set-WebApplicationProxySslCertificate -Thumbprint '<thumbprint of new cert>'` in PowerShell to install a new TLS/SSL certificate for the WAP server.

### Remove Old Certificates

Old certificates that the adversary may use should be removed. This may be accomplished with the following command run on the primary AD FS server. 

`Remove-ADFSCertificate -CertificateType Token-Signing -thumbprint <thumbprint>`

### Verify Certificate Revocation

Administrators should run the PowerShell command `Get -MsolFederationProperty -DomainName` and confirm that thumbprints belonging to previously compromised certificates are not present.

### Update Federation Partners

Ensure all federation partners have consumed the new certificate. Partners that cannot consume federation metadata must be manually sent the public key of the token signing and decrypting certificate.

### Revoke Microsoft 365 Refresh Tokens

After connecting to Azure with `Connect-Azure-AD`, revoke M365 refresh tokens with the following PowerShell command:

`Get-AzureADuser -all $true | Revoke-AzureADUserAllRefreshToken`

For further guidance see CM0077 - Revoke Microsoft 365 Refresh Tokens.

## Associated Techniques

- [T1606.002](https://attack.mitre.org/techniques/T1606/002/) -  Forge Web Credentials: SAML Tokens 

## Related Countermeasures

- CM0050 - Reset Kerberos Ticket Granting Ticket (KRBTGT) Password Twice
- CM0069 - Reset Directory Services Restore Mode (DSRM) Account Passwords on Domain Controllers
- CM0077 - Revoke Microsoft 365 Refresh Tokens
- CM0134 - Audit Federated Systems Trust Settings

## References

- Emergency rotation of the AD FS certificates | <https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-emergency-ad-fs-certificate-rotation>
- Import a Certificate | <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754489(v=ws.11)>
- Manage TLS/SSL certificates in AD FS and WAP in Windows Server 2016 | <https://learn.microsoft.com/en-us/windows-server/identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap#replace-the-tlsssl-certificate-for-ad-fs>
- Remediation and Hardening Strategies for Microsoft 365 to Defend Against UNC2452 | <https://www.mandiant.com/sites/default/files/2021-11/wp-m-unc2452-000343.pdf>
