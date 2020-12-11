---
description: 深入瞭解：安裝受信任的 TPM 根憑證
title: 安裝受信任的 TPM 根憑證
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 06/27/2019
ms.openlocfilehash: 8efc08856c234d55f6cc9b87bade9f5c81bf4332
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049706"
---
# <a name="install-trusted-tpm-root-certificates"></a>安裝受信任的 TPM 根憑證

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

當您設定 HGS 使用 TPM 證明時，您也需要設定 HGS 信任您伺服器中的 Tpm 廠商。
這個額外的驗證程式可確保只有真實可靠的 Tpm 能夠與您的 HGS 證明。
如果您嘗試向註冊不受信任的 TPM `Add-HgsAttestationTpmHost` ，您會收到錯誤，指出 TPM 廠商不受信任。

若要信任您的 Tpm，在您的伺服器 Tpm 中用來簽署簽署金鑰的根和中繼簽署憑證都必須安裝在 HGS 上。
如果您在資料中心內使用一個以上的 TPM 模型，您可能需要為每個模型安裝不同的憑證。
HGS 會查看廠商憑證的「TrustedTPM_RootCA」和「TrustedTPM_IntermediateCA」憑證存放區。

> [!NOTE]
> TPM 廠商憑證不同于 Windows 預設安裝的憑證，並代表 TPM 廠商所使用的特定根和中繼憑證。

Microsoft 會發佈受信任的 TPM 根目錄和中繼憑證集合，以方便您使用。
您可以使用下列步驟來安裝這些憑證。
如果您的 TPM 憑證未包含在下列套件中，請洽詢您的 TPM 廠商或伺服器 OEM，以取得特定 TPM 模型的根和中繼憑證。

在 **每一部 HGS 伺服器** 上重複執行下列步驟：

1.  從下載最新的套件 [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925) 。

2.  驗證 cab 檔案的簽章，以確保其真實性。 如果簽章無效，請勿繼續進行。

    ```powershell
    Get-AuthenticodeSignature .\TrustedTpm.cab
    ```

    以下是一些輸出範例︰

    ```
    Directory: C:\Users\Administrator\Downloads

    SignerCertificate                         Status                                 Path
    -----------------                         ------                                 ----
    0DD6D4D4F46C0C7C2671962C4D361D607E370940  Valid                                  TrustedTpm.cab
    ```

2.  展開 cab 檔。

    ```
    mkdir .\TrustedTPM
    expand.exe -F:* <Path-To-TrustedTpm.cab> .\TrustedTPM
    ```

3.  根據預設，設定腳本會安裝每個 TPM 廠商的憑證。 如果您只想要匯入特定 TPM 廠商的憑證，請刪除您的組織不信任的 TPM 廠商資料夾。

4.  在展開的資料夾中執行安裝腳本，以安裝受信任的憑證套件。

    ```
    cd .\TrustedTPM
    .\setup.cmd
    ```

若要加入新的憑證，或在先前安裝期間刻意略過的憑證，只要在 HGS 叢集中的每個節點上重複上述步驟。
現有的憑證會保持受信任，但是擴充的封包檔中找到的新憑證將會新增至受信任的 TPM 存放區。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [設定網狀架構 DNS](guarded-fabric-configuring-fabric-dns-tpm.md)



