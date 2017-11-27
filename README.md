# Self Signed Certificate Script

This repository hosts the PowerShell script created by Vadims Podans. The original script can be found https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6. 

## Changes made
The script in this repository is modified by adding additional parameter of Issuer Signing Certificate (CA Cert) assumed to be in LocalMachine Personal store.This helps to create a Self-signed CA certificate and then create additional self-signed certificates using the new CA certificate as the root certificate (i.e. signed by this certifcate). The advantage of thsi chaining is that in some cases when many certificates are to be created and trusted on many machines, a single root certificate needs to be trusted and any future self-signed  certificates will be automatically trusted.
