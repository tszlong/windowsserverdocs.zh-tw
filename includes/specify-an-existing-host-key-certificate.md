或者，您可以指定憑證指紋，如果您想要使用您自己的憑證。 這可以是很有用，如果您想要共用憑證跨多部電腦，或使用憑證，繫結到 TPM 或 HSM。 建立 TPM 繫結憑證 （這可防止它擁有的私用金鑰被偷，並且在另一部電腦上使用，而且需要僅 TPM 1.2） 的範例如下：

```powersehll
$tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
```


<!-- Appears in set-up-hgs-for-always-encrypted-in-sql-server.md and guarded-fabric-create-host-key.md
-->