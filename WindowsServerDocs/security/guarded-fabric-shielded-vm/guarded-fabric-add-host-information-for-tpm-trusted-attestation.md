---
title: 新增 TPM 信任證明的主機資訊
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 3647c9708ad68dec0ac13c85fced2b12150ccf60
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447197"
---
>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

### <a name="add-host-information-for-tpm-trusted-attestation"></a>新增 TPM 信任證明的主機資訊

TPM 模式中，網狀架構系統管理員擷取三種類型的主控件的詳細資訊，每個要加入至 HGS 設定的需求：

- 針對每個 HYPER-V 主機的 TPM 識別碼 (EKpub)
- 程式碼完整性原則，允許的二進位檔的 HYPER-V 主機的白名單
- 相同類別的硬體上執行 TPM 基準 （開機測量） 表示一組的 HYPER-V 主機

系統管理員會在 網狀架構之後擷取的資訊，將它新增至 HGS 設定，如下列程序中所述。

1.  取得 XML 檔案，其中包含 EKpub 資訊並將它們複製到一個 HGS 伺服器。 將會有一個 XML 檔案中的每個主機。 然後，在一個 HGS 伺服器上已提升權限的 Windows PowerShell 主控台，執行下列命令。 每個 XML 檔案，重複執行命令。

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > 如果加入不受信任的簽署金鑰憑證 (EKCert) 有關的 TPM 識別碼時，您會遇到錯誤，請確認[受信任的 TPM 根憑證已新增](guarded-fabric-install-trusted-tpm-root-certificates.md)HGS 節點。
    > 此外，有些 TPM 廠商請勿使用 EKCerts。
    > 您可以檢查是否 EKCert 遺漏 [記事本] 之類的編輯器中開啟 XML 檔案，並檢查錯誤訊息，指出沒有 EKCert 找不到。
    > 如果此情況下，而且您信任您的電腦中的 TPM 是真確，，您可以使用`-Force`覆寫這項安全檢查，並將主應用程式識別碼新增到 HGS 的旗標。

2. 取得二進位格式 (*.p7b) 的主機網狀架構系統管理員建立的程式碼完整性原則。 請將它複製到一個 HGS 伺服器。 然後執行下列命令。

    針對`<PolicyName>`，指定描述的主機，它會套用到類型的 CI 原則的名稱。 最佳的作法是品牌/型號機器和在其上執行任何特殊的軟體設定為其命名。<br>針對`<Path>`，指定的路徑和檔案名稱的程式碼完整性原則。

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

3. 取得從參考主機的網狀架構系統管理員擷取 TCGlog 檔案。 將檔案複製到一個 HGS 伺服器。 然後執行下列命令。 一般而言，您將它所代表的硬體類別命名原則時 （例如，"製造商型號版本 」）。

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

如此即完成設定 TPM 模式的 HGS 叢集的程序。 網狀架構系統管理員可能需要您從 HGS 提供兩個 Url，才能完成設定主機。 若要取得 Url，HGS 伺服器上，執行[Get HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps)。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [確認證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)