找出您的 HGS 守護者憑證。 您需要一個簽署憑證和用來初始化 HGS 叢集中的一個加密憑證。
憑證提供給 HGS 的最簡單方式是建立密碼保護的 PFX 檔案的每個憑證，其中包含公開和私密金鑰的金鑰。 如果您使用 hsm 型金鑰或其他非可匯出的憑證，請確定憑證已安裝至本機電腦的憑證存放區，然後再繼續。
如需有關要使用哪個憑證的詳細資訊，請參閱[憑證取得 HGS](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-obtain-certs)。

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->