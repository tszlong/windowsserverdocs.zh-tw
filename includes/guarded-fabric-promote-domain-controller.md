1.  執行[安裝 HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/install-hgsserver)將加入網域和升級網域控制站的節點。

    ```powershell
    $adSafeModePassword = ConvertTo-SecureString -AsPlainText '<password>' -Force

    $cred = Get-Credential 'relecloud\Administrator'

    Install-HgsServer -HgsDomainName 'bastion.local' -HgsDomainCredential $cred -SafeModeAdministratorPassword $adSafeModePassword -Restart
    ```

2.  當伺服器重新開機時，則會登入網域系統管理員帳戶。

<!-- Appears twice in guarded-fabric-configure-additional-hgs-nodes.md 
-->