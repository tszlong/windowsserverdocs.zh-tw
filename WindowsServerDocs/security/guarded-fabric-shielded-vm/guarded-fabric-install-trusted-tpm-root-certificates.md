---
title: 安裝受信任的 TPM 根憑證
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 06/27/2019
ms.openlocfilehash: 096a40f422f308a036b8062e4515ebe698c31f08
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856571"
---
# <a name="install-trusted-tpm-root-certificates"></a>安裝受信任的 TPM 根憑證

>適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

當您設定 HGS 使用 TPM 證明時，您也必須設定 HGS 以信任伺服器中 Tpm 的廠商。
這個額外的驗證程式可確保只有可靠且值得信賴的 Tpm 能夠使用您的 HGS 進行證明。
如果您嘗試使用 `Add-HgsAttestationTpmHost`註冊不受信任的 TPM，您會收到錯誤，指出 TPM 廠商不受信任。

若要信任您的 Tpm，您必須在 HGS 上安裝用來簽署伺服器 Tpm 中簽署金鑰的根和中繼簽署憑證。
如果您在資料中心內使用一個以上的 TPM 模型，可能需要為每個模型安裝不同的憑證。
HGS 會查看廠商憑證的「TrustedTPM_RootCA」和「TrustedTPM_IntermediateCA」憑證存放區。

> [!NOTE]
> TPM 廠商憑證不同于 Windows 中預設安裝的憑證，並代表 TPM 廠商所使用的特定根和中繼憑證。

Microsoft 會發佈受信任的 TPM 根和中繼憑證的集合，以方便您使用。
您可以使用下列步驟來安裝這些憑證。
如果您的 TPM 憑證未包含在下列套件中，請洽詢您的 TPM 廠商或伺服器 OEM，以取得特定 TPM 模型的根和中繼憑證。

在**每一部 HGS 伺服器**上重複執行下列步驟：

1.  從[https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925)下載最新的套件。

2.  確認 cab 檔案的簽章，以確保其真實性。 如果簽章無效，請勿繼續進行。

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

2.  展開 cab 檔案。

    ```
    mkdir .\TrustedTPM
    expand.exe -F:* <Path-To-TrustedTpm.cab> .\TrustedTPM
    ```

3.  根據預設，設定腳本會安裝每個 TPM 廠商的憑證。 如果您只想要匯入特定 TPM 廠商的憑證，請刪除組織不信任的 TPM 廠商資料夾。

4.  在擴充的資料夾中執行安裝腳本，以安裝受信任的憑證封裝。

    ```
    cd .\TrustedTPM
    .\setup.cmd
    ```

若要新增新的憑證，或在稍早的安裝期間刻意略過，請直接在 HGS 叢集中的每個節點上重複上述步驟。
現有的憑證會保持受信任，但是在展開的 cab 檔案中找到的新憑證將會新增至受信任的 TPM 存放區。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [設定網狀架構 DNS](guarded-fabric-configuring-fabric-dns-tpm.md)



