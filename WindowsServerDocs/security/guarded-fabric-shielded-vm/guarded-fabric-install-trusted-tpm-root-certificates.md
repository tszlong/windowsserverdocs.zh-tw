---
title: 安裝受信任的 TPM 根憑證
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 09/11/2018
ms.openlocfilehash: 65c5eb25f7a98e4db6a44e705a2968f185f3f962
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447387"
---
# <a name="install-trusted-tpm-root-certificates"></a>安裝受信任的 TPM 根憑證

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

當您設定 HGS 使用 TPM 證明時，您也需要設定 HGS 信任您的伺服器內的 Tpm 的廠商。
此額外的驗證程序可確保能夠與您的 HGS 證明只真實、 可信任的 Tpm。
如果您嘗試註冊不受信任的 TPM 搭配`Add-HgsAttestationTpmHost`，您會收到錯誤，指出不受信任的 TPM 廠商。

若要信任您的 Tpm，根和中繼簽署的憑證，用來簽署在您的伺服器 Tpm 簽署金鑰需要 HGS 上安裝。
如果您使用一個以上的 TPM 模型中您的資料中心時，您可能需要安裝不同的憑證，針對每個模型。
HGS 會查看 「 TrustedTPM_RootCA"及"TrustedTPM_IntermediateCA 」 憑證存放區廠商憑證。

> [!NOTE]
> TPM 廠商憑證不同於預設安裝在 Windows 中，並且表示特定根和 TPM 廠商使用的中繼憑證。

Microsoft 發佈受信任的 TPM 根和中繼憑證的集合，讓您方便參考。
您可以使用下列步驟來安裝這些憑證。
如果 TPM 憑證不會納入下列封裝，請連絡您的 TPM 廠商或 OEM 伺服器取得根和中繼憑證，為您特定的 TPM 模型。

在重複下列步驟**每一部 HGS 伺服器**:

1.  下載最新的封裝，從[ https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab ](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab)。

2.  展開封包檔。

    ```
    mkdir .\TrustedTPM
    expand.exe -F:* <Path-To-TrustedTpm.cab> .\TrustedTPM
    ```

3.  根據預設，設定指令碼會為每個 TPM 廠商安裝憑證。 如果您只想要針對特定 TPM 廠商匯入憑證，請為您的組織不信任的 TPM 廠商刪除的資料夾。

4.  展開資料夾中執行安裝指令碼，以安裝受信任的憑證封裝。

    ```
    cd .\TrustedTPM
    .\setup.cmd
    ```

若要加入新的憑證或故意略過在更早版本的安裝期間，只要重複上述步驟，在您的 HGS 叢集中的每個節點上。
現有的憑證仍受信任，但在展開的封包檔中找到的新憑證將會新增至受信任的 TPM 儲存。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [設定網狀架構 DNS](guarded-fabric-configuring-fabric-dns-tpm.md)



