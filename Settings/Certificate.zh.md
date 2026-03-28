# 签名证书

ContextMenuX 将右键菜单项打包为 MSIX 包，需要代码签名证书。

## 证书规格

用于签署 MSIX 包的证书必须满足以下要求：

| 参数 | 值 |
|------|-----|
| 算法 | RSA 2048 位 |
| 哈希 | SHA256（PKCS#1 v1.5 填充） |
| 密钥用途 | 数字签名（DigitalSignature） |
| 增强密钥用途 | 代码签名（`1.3.6.1.5.5.7.3.3`） |
| 基本约束 | 非 CA |
| 主体 | `CN=ModernMenuSparseAppx` |

如果你要创建自己的证书，请确保它符合上述参数——特别是**主体 CN** 和**代码签名 EKU**。

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

然后从证书存储中导出：

```powershell
# 查找证书
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object { $_.Subject -eq "CN=ModernMenuSparseAppx" }

# 导出 PFX（含私钥）
$password = ConvertTo-SecureString -String "12345678" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath "TemporarySelfSignKey.pfx" -Password $password

# 导出 CER（仅公钥）
Export-Certificate -Cert $cert -FilePath "TemporarySelfSignKey.cer"
```

### OpenSSL

```bash
# 生成私钥和自签名证书
openssl req -x509 -newkey rsa:2048 -sha256 -days 1825 \
    -keyout key.pem -out cert.pem \
    -subj "/CN=ModernMenuSparseAppx" \
    -addext "basicConstraints=CA:FALSE" \
    -addext "keyUsage=digitalSignature" \
    -addext "extendedKeyUsage=codeSigning" \
    -addext "subjectAltName=DNS:ModernMenuSparseAppx" \
    -addext "subjectKeyIdentifier=hash"

# 转换为 PFX
openssl pkcs12 -export -out TemporarySelfSignKey.pfx \
    -inkey key.pem -in cert.pem -password pass:12345678

# 导出 CER（DER 格式）
openssl x509 -in cert.pem -outform der -out TemporarySelfSignKey.cer
```

## 生成

点击**生成**创建符合上述规格的自签名证书（有效期 5 年）。这是最快的入门方式。

生成后，你需要**将证书安装为受信任证书**（需要管理员权限），Windows 才会接受已签名的包。

## 导入

如果你更倾向于使用自己的证书，请导入包含以下文件的文件夹：
- `.pfx` 文件（用于签名的私钥，密码：`12345678`）
- `.cer` 文件（用于信任的公钥证书）

导入的证书必须具有相同的主体 CN（`CN=ModernMenuSparseAppx`）和代码签名 EKU。

## 证书详情

设置页面显示：
- **来源** — 生成或导入
- **信任状态** — 证书是否已安装到系统信任存储
- **指纹** — 证书标识符
- **文件位置** — PFX/CER 文件的存储路径

## 安装信任

点击**安装信任（管理员）**将证书添加到 Windows 的受信任存储。这是 MSIX 包成功安装的必要步骤。

> **注意：** 每个证书只需执行一次此操作。如果你重新生成或导入了新证书，需要再次安装信任。
