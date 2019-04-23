---
title: 建立主應用程式金鑰，並將它新增至 HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 82a561d51e9a396dcdc86ee3e37a8d7ffb8efd6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870779"
---
# <a name="create-a-host-key-and-add-it-to-hgs"></a>建立主應用程式金鑰，並將它新增至 HGS

>適用於：Windows Server 2019


本主題涵蓋如何準備要做為受防護的主機使用主機金鑰證明 （索引鍵模式） 的 HYPER-V 主機。 您會建立主應用程式的金鑰組 （或使用現有的憑證），並將 HGS 金鑰的公開部分。

## <a name="create-a-host-key"></a>建立主索引鍵

1.  HYPER-V 主機電腦上安裝 Windows Server 2019。
2.  安裝 HYPER-V 和 主機守護者 HYPER-V 支援功能：

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ``` 

3.  自動產生的主索引鍵，或選取現有的憑證。 如果您使用自訂憑證，它應該有至少 2048年位元 RSA 金鑰、 用戶端驗證 EKU 和數位簽章金鑰使用方法。

    ```powershell
    Set-HgsClientHostKey
    ```

    或者，您可以指定憑證指紋，如果您想要使用您自己的憑證。 
    這可以是很有用，如果您想要共用憑證跨多部電腦，或使用憑證，繫結到 TPM 或 HSM。 建立 TPM 繫結憑證 （這可防止它擁有的私用金鑰被偷，並且在另一部電腦上使用，而且需要僅 TPM 1.2） 的範例如下：

    ```powershell
    $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
    Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
    ```

4.  取得公用下半部 HGS 伺服器提供的索引鍵。 您可以使用下列 cmdlet 或者，如果您有儲存在其他地方，憑證提供.cer，其中包含公用金鑰的下半部。 請注意，我們只會儲存並驗證 HGS; 上的公開金鑰我們不需要保留任何憑證資訊也不在我們驗證憑證鏈結或到期日。

    ```powershell
    Get-HgsClientHostKey -Path "C:\temp\$env:hostname-HostKey.cer"
    ```

5.  將.cer 檔案複製到您的 HGS 伺服器。

## <a name="add-the-host-key-to-the-attestation-service"></a>將主應用程式金鑰新增至證明服務

此步驟會對 HGS 伺服器，並可讓主機執行受防護的 Vm。 建議您將名稱設定為 FQDN，或上已安裝的主機電腦，因此您可以輕鬆地參考哪一部主機的索引鍵的資源識別碼。

```powershell
Add-HgsAttestationHostKey -Name MyHost01 -Path "C:\temp\MyHost01-HostKey.cer"
``` 

## <a name="next-step"></a>後續步驟

>[!div class="nextstepaction"]
[確認主機可以證明成功](guarded-fabric-confirm-hosts-can-attest-successfully.md)

## <a name="see-also"></a>另請參閱

- [部署主機守護者服務的受防護主機和受防護的 Vm](guarded-fabric-deploying-hgs-overview.md)
