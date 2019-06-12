---
title: 初始化在防禦樹系中使用 TPM 模式 HGS 叢集
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 458642eedbfdc94ef0f3d6f6fe08ed4ead475ab0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447437"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-an-existing-bastion-forest"></a>初始化 HGS 叢集中現有的防禦樹系中使用 TPM 模式

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

Active Directory 網域服務將會安裝在電腦上，但應保持未設定。

[!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

在繼續之前，請確定您已預先設置的主機守護者服務的叢集物件，並授與登入的使用者**完全控制**透過 Active Directory 中 VCO 和 CNO 的物件。
虛擬電腦物件名稱必須傳遞給`-HgsServiceName`參數，而叢集名稱以`-ClusterName`參數。

> [!TIP]
> 再次檢查您的 AD 網域控制站，以確保您的叢集物件已複寫到所有網域控制站然後再繼續。

如果您使用 PFX 憑證，請在 HGS 伺服器上執行下列命令：

```powershell
$signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
$encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

Install-ADServiceAccount -Identity 'HGSgMSA'

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
```

如果您使用的憑證安裝在本機電腦 （例如 hsm 型憑證和非可匯出的憑證） 上，使用`-SigningCertificateThumbprint`和`-EncryptionCertificateThumbprint`參數改。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [安裝 TPM 根憑證](guarded-fabric-install-trusted-tpm-root-certificates.md)