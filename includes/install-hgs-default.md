主機守護者服務應該安裝在不同的 Active Directory 樹系中。
請確定您的 HGS 機器**不**開始之前先在本機電腦發票的身分登入加入網域。

執行下列命令來安裝 「 主機守護者服務，並設定它的網域。
您在此處指定的密碼只會套用到目錄服務修復模式密碼適用於 Active Directory;它會*不*變更您的系統管理員帳戶登入密碼。
您可以提供-HgsDomainName 您選擇任何網域的名稱。

```powershell
$adminPassword = ConvertTo-SecureString -AsPlainText '<password>' -Force

Install-HgsServer -HgsDomainName 'bastion.local' -SafeModeAdministratorPassword $adminPassword -Restart
```

<!-- Appears in guarded-fabric-install-hgs-default.md and set-up-hgs-for-always-encrypted-in-sql-server.md
-->
