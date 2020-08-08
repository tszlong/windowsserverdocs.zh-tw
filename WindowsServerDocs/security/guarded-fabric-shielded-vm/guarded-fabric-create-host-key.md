---
title: 建立主機金鑰並將它新增至 HGS
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 4572c6424f018d711a18d5ee763b00a253a73529
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961602"
---
# <a name="create-a-host-key-and-add-it-to-hgs"></a>建立主機金鑰並將它新增至 HGS

>適用於：Windows Server 2019

本主題說明如何使用主機金鑰證明 (金鑰模式) ，將 Hyper-v 主機準備成為受防護主機。 您將建立主機金鑰組 (或使用現有的憑證) 並將金鑰的公用部分新增至 HGS。

## <a name="create-a-host-key"></a>建立主機金鑰

1. 在您的 Hyper-v 主機電腦上安裝 Windows Server 2019。
2. 安裝 Hyper-v 和主機守護者 Hyper-v 支援功能：

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

3. 自動產生主機金鑰，或選取現有的憑證。 如果您使用自訂憑證，它應該至少有2048位的 RSA 金鑰、用戶端驗證 EKU 和數位簽章金鑰使用方式。

    ```powershell
    Set-HgsClientHostKey
    ```

    或者，如果您想要使用自己的憑證，可以指定指紋。
    如果您想要跨多部電腦共用憑證，或使用系結至 TPM 或 HSM 的憑證，這會很有用。 以下是建立 TPM 系結憑證 (的範例，可防止它在另一部電腦上竊取和使用私密金鑰，而且只需要 TPM 1.2) ：

    ```powershell
    $tpmBoundCert = New-SelfSignedCertificate -Subject "Host Key Attestation ($env:computername)" -Provider "Microsoft Platform Crypto Provider"
    Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
    ```

4. 取得金鑰的公開半部，以提供給 HGS 伺服器。 您可以使用下列 Cmdlet，或者，如果您將憑證儲存在其他位置，請提供 .cer，其中包含金鑰的公用一半。 請注意，我們只會儲存和驗證 HGS 上的公開金鑰;我們不會保留任何憑證資訊，也不會驗證憑證鏈或到期日。

    ```powershell
    Get-HgsClientHostKey -Path "C:\temp\$env:hostname-HostKey.cer"
    ```

5. 將 .cer 檔案複製到您的 HGS 伺服器。

## <a name="add-the-host-key-to-the-attestation-service"></a>將主機金鑰新增至證明服務

此步驟是在 HGS 伺服器上完成，可讓主機執行受防護的 Vm。 建議您將名稱設定為主電腦的 FQDN 或資源識別碼，以便您可以輕鬆地參考安裝金鑰的主機。

```powershell
Add-HgsAttestationHostKey -Name MyHost01 -Path "C:\temp\MyHost01-HostKey.cer"
```

## <a name="next-step"></a>後續步驟

- [確認主機可以成功證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)

## <a name="additional-references"></a>其他參考資料

- [部署受防護主機和受防護 VM 的主機守護者服務](guarded-fabric-deploying-hgs-overview.md)
