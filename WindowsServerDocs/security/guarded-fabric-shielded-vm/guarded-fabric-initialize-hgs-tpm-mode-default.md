---
description: '深入瞭解：在新的專用樹系中使用 TPM 模式初始化 HGS 叢集 (預設) '
title: '在新的專用樹系中使用 TPM 模式初始化 HGS 叢集 (預設) '
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: eb63af4feb13b519906b8898a1eb9a352bf91d27
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047336"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-a-new-dedicated-forest-default"></a>在新的專用樹系中使用 TPM 模式初始化 HGS 叢集 (預設) 

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  在第一個 HGS 節點上提高許可權的 PowerShell 視窗中執行 [Initialize HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/initialize-hgsserver) 。 此 Cmdlet 的語法支援許多不同的輸入，但2個最常見的調用如下：

    -   如果您要針對簽署和加密憑證使用 PFX 檔案，請執行下列命令：

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
        ```

    -   如果您使用本機憑證存放區中安裝的不可匯出憑證，請執行下列命令。 如果您不知道憑證的指紋，您可以藉由執行來列出可用的憑證 `Get-ChildItem Cert:\LocalMachine\My` 。

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' -TrustTpm
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [安裝 TPM 根憑證](guarded-fabric-install-trusted-tpm-root-certificates.md)
