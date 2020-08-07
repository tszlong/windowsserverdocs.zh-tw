---
title: '使用新的專用樹系中的金鑰模式，將 HGS 叢集初始化 (預設) '
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 9098c339207464df3bf2eefa0e18250d2356a072
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950903"
---
# <a name="initialize-the-hgs-cluster-using-key-mode-in-a-new-dedicated-forest-default"></a>使用新的專用樹系中的金鑰模式，將 HGS 叢集初始化 (預設) 

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016


1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]
2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  在第一個 HGS 節點上已提升許可權的 PowerShell 視窗中執行[Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx) 。 此 Cmdlet 的語法支援許多不同的輸入，但2個最常見的調用如下：

    -   如果您使用 PFX 檔案作為簽署和加密憑證，請執行下列命令：

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustHostkey
        ```

    -   如果您使用的是安裝在本機憑證存放區中的不可匯出憑證，請執行下列命令。 如果您不知道憑證的指紋，您可以執行來列出可用的憑證 `Get-ChildItem Cert:\LocalMachine\My` 。

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' --TrustHostKey
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]


## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [建立主機金鑰](guarded-fabric-create-host-key.md)