---
title: 初始化新的專用樹系 （預設值） 在使用 TPM 模式 HGS 叢集
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 6d4232b0b248aba8b64f31ac28db1480c14db53d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863869"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-a-new-dedicated-forest-default"></a>初始化新的專用樹系 （預設值） 在使用 TPM 模式 HGS 叢集

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  執行[Initialize HgsServer](https://technet.microsoft.com/library/mt652185.aspx)第一個 HGS 節點上提高權限的 PowerShell 視窗中。 此 cmdlet 的語法支援許多不同的輸入，但 2 個最常見的引動過程如下：

    -   如果您要用於簽署和加密憑證的 PFX 檔案，請執行下列命令：

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
        ```

    -   如果您使用非可匯出的憑證安裝在本機憑證存放區中，執行下列命令。 如果您不知道您的憑證指紋，您可以執行來列出可用的憑證`Get-ChildItem Cert:\LocalMachine\My`。

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' -TrustTpm
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]

## <a name="next-step"></a>後續步驟

>[!div class="nextstepaction"]
[安裝 TPM 根憑證](guarded-fabric-install-trusted-tpm-root-certificates.md)
  