# Signing Certificate

ContextMenuX packages context menu items as MSIX packages, which require a code-signing certificate.

## Certificate Specification

The certificate used to sign MSIX packages must meet the following requirements:

| Parameter | Value |
|-----------|-------|
| Algorithm | RSA 2048-bit |
| Hash | SHA256 (PKCS#1 v1.5 padding) |
| Key Usage | DigitalSignature |
| Enhanced Key Usage | Code Signing (`1.3.6.1.5.5.7.3.3`) |
| Basic Constraints | Not a CA |
| Subject | `CN=ModernMenuSparseAppx` |

If you want to create your own certificate, make sure it matches the above parameters — especially the **Subject CN** and **Code Signing EKU**.

### PowerShell

```powershell
New-SelfSignedCertificate `
    -Type Custom `
    -Subject "CN=ModernMenuSparseAppx" `
    -KeyUsage DigitalSignature `
    -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3") `
    -KeyAlgorithm RSA `
    -KeyLength 2048 `
    -HashAlgorithm SHA256 `
    -CertStoreLocation "Cert:\CurrentUser\My" `
    -NotAfter (Get-Date).AddYears(5)
```

Then export from the certificate store:

```powershell
# Find the cert
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object { $_.Subject -eq "CN=ModernMenuSparseAppx" }

# Export PFX (with private key)
$password = ConvertTo-SecureString -String "12345678" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath "TemporarySelfSignKey.pfx" -Password $password

# Export CER (public only)
Export-Certificate -Cert $cert -FilePath "TemporarySelfSignKey.cer"
```

### OpenSSL

```bash
# Generate private key and self-signed certificate
openssl req -x509 -newkey rsa:2048 -sha256 -days 1825 \
    -keyout key.pem -out cert.pem \
    -subj "/CN=ModernMenuSparseAppx" \
    -addext "basicConstraints=CA:FALSE" \
    -addext "keyUsage=digitalSignature" \
    -addext "extendedKeyUsage=codeSigning" \
    -addext "subjectAltName=DNS:ModernMenuSparseAppx" \
    -addext "subjectKeyIdentifier=hash"

# Convert to PFX
openssl pkcs12 -export -out TemporarySelfSignKey.pfx \
    -inkey key.pem -in cert.pem -password pass:12345678

# Export CER (DER format)
openssl x509 -in cert.pem -outform der -out TemporarySelfSignKey.cer
```

## Generate

Click **Generate** to create a self-signed certificate matching the above spec (valid for 5 years). This is the quickest way to get started.

After generating, you need to **install the certificate as trusted** (requires Admin) so Windows accepts the signed packages.

## Import

If you prefer to use your own certificate, import a folder containing:
- A `.pfx` file (private key for signing, password: `12345678`)
- A `.cer` file (public certificate for trust)

The imported certificate must have the same Subject CN (`CN=ModernMenuSparseAppx`) and Code Signing EKU.

## Certificate Details

The settings page shows:
- **Source** — Generated or Imported
- **Trust status** — Whether the certificate is installed in the system trust store
- **Thumbprint** — Certificate identifier
- **File location** — Where the PFX/CER files are stored

## Install Trust

Click **Install Trust (Admin)** to add the certificate to Windows' trusted store. This is required for the MSIX packages to install successfully.

> **Note:** You only need to do this once per certificate. If you regenerate or import a new certificate, you'll need to install trust again.
