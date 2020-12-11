---
description: '深入瞭解：使用新的專用樹系中的金鑰模式初始化 HGS 叢集 (預設值) '
title: '使用新的專用樹系中的金鑰模式初始化 HGS 叢集 (預設) '
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: b42535dbaa58b53e40aee555f1fdb47a24e94355
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049766"
---
# <a name="initialize-the-hgs-cluster-using-key-mode-in-a-new-dedicated-forest-default"></a>使用新的專用樹系中的金鑰模式初始化 HGS 叢集 (預設) 

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016


1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]
2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  在第一個 HGS 節點上提高許可權的 PowerShell 視窗中執行 [Initialize HgsServer](https://technet.microsoft.com/library/mt652185.aspx) 。 此 Cmdlet 的語法支援許多不同的輸入，但2個最常見的調用如下：

    -   如果您要針對簽署和加密憑證使用 PFX 檔案，請執行下列命令：

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustHostkey
        ```

    -   如果您使用本機憑證存放區中安裝的不可匯出憑證，請執行下列命令。 如果您不知道憑證的指紋，您可以藉由執行來列出可用的憑證 `Get-ChildItem Cert:\LocalMachine\My` 。

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' --TrustHostKey
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]


## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [建立主機金鑰](guarded-fabric-create-host-key.md)
