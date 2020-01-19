---
title: 新增 TPM 信任證明的主機資訊
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 06/21/2019
ms.openlocfilehash: 923bc2c46f37cf7e631a744c9eae85c3dd7506dd
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949810"
---
>適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

# <a name="add-host-information-for-tpm-trusted-attestation"></a>新增 TPM 信任證明的主機資訊

針對 TPM 模式，網狀架構系統管理員會捕獲三種主機資訊，每個都必須新增至 HGS 設定：

- 每部 Hyper-v 主機的 TPM 識別碼（EKpub）
- 程式碼完整性原則，這是 Hyper-v 主機允許之二進位檔的允許清單
- TPM 基準（開機測量），代表在相同硬體類別上執行的一組 Hyper-v 主機

在網狀架構系統管理員捕捉到資訊之後，請將它新增至 HGS 設定，如下列程式所述。

1. 取得包含 EKpub 資訊的 XML 檔案，並將其複製到 HGS 伺服器。 每一部主機會有一個 XML 檔案。 然後，在 HGS 伺服器上提高許可權的 Windows PowerShell 主控台中，執行下列命令。 針對每個 XML 檔案重複此命令。

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > 如果您在新增與不受信任的簽署金鑰憑證（EKCert）有關的 TPM 識別碼時遇到錯誤，請確定已將[受信任的 TPM 根憑證新增](guarded-fabric-install-trusted-tpm-root-certificates.md)至 HGS 節點。
    > 此外，某些 TPM 廠商不會使用 EKCerts。
    > 您可以在 [記事本] 之類的編輯器中開啟 XML 檔案，並檢查是否有指出找不到 EKCert 的錯誤訊息，以檢查是否遺漏 EKCert。
    > 如果是這種情況，而且您信任電腦中的 TPM 是真實的，您可以使用 `-Force` 旗標來覆寫此安全性檢查，並將主機識別碼新增至 HGS。

2. 取得網狀架構系統管理員為主機建立的程式碼完整性原則（二進位格式（\*. p7b）。 將它複製到 HGS 伺服器。 然後執行下列命令。

    針對 `<PolicyName>`，請指定 CI 原則的名稱，以描述其適用的主機類型。 最佳做法是在您的機器的製作/型號和其上執行的任何特殊軟體設定之後，將它命名為。<br>針對 `<Path>`，請指定程式碼完整性原則的路徑和檔案名。

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```
    
    > [!NOTE]
    > 如果您使用的是已簽署的程式碼完整性原則，請向 HGS 註冊相同原則的不帶正負號複本。
    > 程式碼完整性原則的簽章可用來控制原則的更新，但不會測量到主機 TPM，因此 HGS 無法將其證明至。

3. 取得網狀架構系統管理員從參照主機所捕獲的 TCGlog 檔案。 將檔案複製到 HGS 伺服器。 然後執行下列命令。 一般來說，您會在它所代表的硬體類別之後，將原則命名為（例如，「製造商型號修訂」）。

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

這會完成為 TPM 模式設定 HGS 叢集的程式。 網狀架構系統管理員可能會要求您從 HGS 提供兩個 Url，然後才能完成主機的設定。 若要取得這些 Url，請在 HGS 伺服器上執行[HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps)。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [確認證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)
