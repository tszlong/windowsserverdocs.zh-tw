首先，取得 HGS 從您的憑證授權單位的 SSL 憑證。 每個主機電腦必須信任的 SSL 憑證，因此建議您從貴公司的公用金鑰基礎結構或協力廠商 CA 發出的 SSL 憑證。 IIS 所支援的任何 SSL 憑證已受到 HGS，不過**上憑證的主體名稱必須符合完整的 HGS 服務名稱**（叢集分散式的網路名稱）。 比方說，如果 HGS 網域是"bastion.local 」，而且您的 HGS 服務名稱是"hgs 」，您的 SSL 憑證應發出的"hgs.bastion.local 」。 您可以將額外的 DNS 名稱新增至憑證的主體替代名稱 欄位中如有必要。

一旦您的 SSL 憑證資訊，請開啟提升權限的 Powershell 工作階段，而且是在執行時提供的憑證路徑[組 HgsServer](https://technet.microsoft.com/itpro/powershell/windows/host-guardian-service/server/set-hgsserver):


```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

或者，如果您已安裝憑證的本機憑證存放區，您可以參考其憑證指紋：

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

> [!IMPORTANT]
> 設定 HGS 使用 SSL 憑證不會停用 HTTP 端點。
> 如果您想要只允許使用 HTTPS 端點，設定 Windows 防火牆封鎖連接埠 80 的傳入的連接。
> **請勿修改 IIS 繫結**HGS 網站移除 HTTP 端點; 它不支援這麼做。
