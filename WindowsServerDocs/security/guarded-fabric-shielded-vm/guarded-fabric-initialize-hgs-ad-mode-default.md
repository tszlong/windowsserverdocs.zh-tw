---
title: 在新的專用樹系中使用 AD 模式初始化 HGS 叢集（預設值）
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4dd10efecf391f7087962e514db7a59135bd93e8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403651"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-a-new-dedicated-forest-default"></a>在新的專用樹系中使用 AD 模式初始化 HGS 叢集（預設值）

>適用於：Windows Server (半年度管道)、Windows Server 2016

>[!IMPORTANT]
>從 Windows Server 2019 開始，系統管理員信任的證明（AD 模式）已淘汰。 針對不可能進行 TPM 證明的環境，請設定[主機金鑰證明](guarded-fabric-initialize-hgs-key-mode-default.md)。 主機金鑰證明提供與 AD 模式類似的保證，而且設定起來較簡單。 

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)] 
2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  在第一個 HGS 節點上已提升許可權的 PowerShell 視窗中執行[Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx) 。 此 Cmdlet 的語法支援許多不同的輸入，但2個最常見的調用如下：

    -   如果您使用 PFX 檔案作為簽署和加密憑證，請執行下列命令：

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
        ```

    -   如果您使用的是安裝在本機憑證存放區中的不可匯出憑證，請執行下列命令。 如果您不知道憑證的指紋，可以藉由執行 `Get-ChildItem Cert:\LocalMachine\My` 來列出可用的憑證。

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' --TrustActiveDirectory
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]  

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [設定網狀架構 DNS](guarded-fabric-configuring-fabric-dns-ad.md)