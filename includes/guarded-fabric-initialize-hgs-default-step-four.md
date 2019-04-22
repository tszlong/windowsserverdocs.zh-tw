如果您提供任何憑證給 HGS 使用指紋，我們將會指示授與 HGS 讀取權限，這些憑證的私密金鑰。 在伺服器上已安裝桌面體驗，請完成下列步驟：

1.  開啟 本機電腦憑證管理員 (**certlm.msc**)
2.  尋找憑證 > 以滑鼠右鍵按一下 > 所有的工作 > 管理私用金鑰
3.  按一下 [新增]。
4.  在 [物件選擇器] 視窗中，按一下**物件類型**並啟用**服務帳戶**
5.  輸入中的警告文字中所述的服務帳戶的名稱 `Initialize-HgsServer`
6.  請確定 gMSA 具有 「 讀取 」 存取私密金鑰。

在 server core 上，您必須下載 PowerShell 模組，以協助設定私用金鑰的權限。

1.  執行`Install-Module GuardedFabricTools`HGS 伺服器有網際網路連線或執行`Save-Module GuardedFabricTools`另一部電腦上複製 HGS 伺服器移轉模組。
2.  執行 `Import-Module GuardedFabricTools`。 這會新增其他屬性，以在 PowerShell 中找到的憑證物件。
3.  在 PowerShell 中尋找您的憑證指紋 `Get-ChildItem Cert:\LocalMachine\My`
4.  更新 ACL，請取代您自己的指紋和下列程式碼的 gMSA 帳戶與帳戶的警告文字中所列`Initialize-HgsServer`。

    ```powershell
    $certificate = Get-Item "Cert:\LocalMachine\1A2B3C..."
    $certificate.Acl = $certificate.Acl | Add-AccessRule "HgsSvc_1A2B3C" Read Allow
    ```

如果您使用 HSM 支援的憑證或憑證儲存在第三方金鑰儲存提供者，這些步驟可能不適用於您。 請參閱您的金鑰儲存提供者的文件，以了解如何管理您的私人金鑰的權限。 在某些情況下，無法使用授權，或安裝憑證時，將會提供給整部電腦的授權。

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->