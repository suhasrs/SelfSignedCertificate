# Self Signed Certificate Script

This repository hosts the PowerShell script created by Vadims Podans. The original script can be found at: https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6.

## Changes made to the original script
The script in this repository is modified by adding additional parameter of Issuer Signing Certificate (CA Cert) assumed to be in LocalMachine Personal store.This helps to create a Self-signed CA certificate and then create additional self-signed certificates using the new CA certificate as the root certificate (i.e. signed by this certifcate). 

The advantage of thsi chaining is that in some cases when many certificates are to be created and trusted on many machines, a single root certificate needs to be trusted and any future self-signed  certificates will be automatically trusted.

## Example
Below is an example of using the PowerShell module to create Self Signed CA Cert and create more chained self-signed certificates.

```
import-module .\New-SelfSignedCertificateEx.ps1

New-SelfsignedCertificateEx -FriendlyName "**Test Root CA**" -Subject "CN=Test Root CA, OU=Test, O=Company, L=IN, S=IN, C=IN" `
-IsCA $true -StoreLocation "LocalMachine" -NotAfter $([datetime]::now.AddYears(10)) -Exportable -SignatureAlgorithm SHA256

New-SelfsignedCertificateEx -FriendlyName "www.domain.com" -Subject "CN=www.domain.com" -SAN "www.domain.com","*.domain2.com" `
-EKU "Server Authentication" -KeyUsage "KeyEncipherment, DigitalSignature" -SignatureAlgorithm SHA256 `
-StoreLocation "LocalMachine" -NotAfter $([datetime]::now.AddYears(10)) -IssuerCertFriendlyName "**Test Root CA**" -Exportable
```
As shown in the above example, "IssuerCertFriendlyName" parameter of creating certificate and "FriendlyName" parameter of creating CA certificate must match.

Feedback/PRs welcome.
