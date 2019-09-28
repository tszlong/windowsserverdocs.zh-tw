---
title: 建立主機金鑰並將它新增至 HGS
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2aea6c8416a0f3af04ad6056c5d09a4d07708eaa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386649"
---
# <a name="create-a-host-key-and-add-it-to-hgs"></a>建立主機金鑰並將它新增至 HGS

>適用於：Windows Server Standard 2012 R2


本主題涵蓋如何使用主機金鑰證明（金鑰模式）準備 Hyper-v 主機成為受防護主機。 您將建立主機金鑰組（或使用現有的憑證），並將金鑰的公用部分新增至 HGS。

## <a name="create-a-host-key"></a>建立主機金鑰

1.  在您的 Hyper-v 主機電腦上安裝 Windows Server 2019。
2.  安裝 Hyper-v 和主機守護者 Hyper-v 支援功能：

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ``` 

3.  自動產生主機金鑰，或選取現有的憑證。 如果您使用自訂憑證，它應該至少有2048位的 RSA 金鑰、用戶端驗證 EKU 和數位簽章金鑰使用方式。

    ```powershell
    Set-HgsClientHostKey
    ```

    或者，如果您想要使用自己的憑證，可以指定指紋。 
    如果您想要跨多部電腦共用憑證，或使用系結至 TPM 或 HSM 的憑證，這會很有用。 以下是建立 TPM 系結憑證的範例（這會使私密金鑰無法被盜用，並在另一部電腦上使用，而且只需要 TPM 1.2）：

    ```powershell
    $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
    Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
    ```

4.  取得金鑰的公開半部，以提供給 HGS 伺服器。 您可以使用下列 Cmdlet，或者，如果您將憑證儲存在其他位置，請提供 .cer，其中包含金鑰的公用一半。 請注意，我們只會儲存和驗證 HGS 上的公開金鑰;我們不會保留任何憑證資訊，也不會驗證憑證鏈或到期日。

    ```powershell
    Get-HgsClientHostKey -Path "C:\temp\$env:hostname-HostKey.cer"
    ```

5.  將 .cer 檔案複製到您的 HGS 伺服器。

## <a name="add-the-host-key-to-the-attestation-service"></a>將主機金鑰新增至證明服務

此步驟是在 HGS 伺服器上完成，可讓主機執行受防護的 Vm。 建議您將名稱設定為主電腦的 FQDN 或資源識別碼，以便您可以輕鬆地參考安裝金鑰的主機。

```powershell
Add-HgsAttestationHostKey -Name MyHost01 -Path "C:\temp\MyHost01-HostKey.cer"
``` 

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [確認主機可以成功證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)

## <a name="see-also"></a>另請參閱

- [為受防護主機和受防護的 Vm 部署主機守護者服務](guarded-fabric-deploying-hgs-overview.md)
