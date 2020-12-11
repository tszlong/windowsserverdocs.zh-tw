---
description: 深入瞭解：使用現有的防禦樹系中的金鑰模式初始化 HGS 叢集
title: 在防禦樹系中使用金鑰模式初始化 HGS 叢集
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 4556c6ba83748e14f2383c1006dfe372b65f4d40
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047326"
---
# <a name="initialize-the-hgs-cluster-using-key-mode-in-an-existing-bastion-forest"></a>使用現有防禦樹系中的金鑰模式來初始化 HGS 叢集

> 適用於：Windows Server 2019
>
> [!div class="step-by-step"]
> [«在新樹](guarded-fabric-install-hgs-in-a-bastion-forest.md) 
>  系中安裝 HGS[建立主機金鑰»](guarded-fabric-create-host-key.md)

Active Directory Domain Services 將安裝在電腦上，但應該保持未設定狀態。

[!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

繼續之前，請確定您已為主機守護者服務預先設置叢集物件，並授與登入的使用者對 Active Directory 中的 VCO 和 CNO 物件的 **完整控制權** 。
必須將虛擬電腦物件名稱傳遞給 `-HgsServiceName` 參數，並將叢集名稱傳遞給 `-ClusterName` 參數。

> [!TIP]
> 再次檢查您的 AD 網域控制站，以確保您的叢集物件已複寫至所有 Dc，然後再繼續。

如果您使用以 PFX 為基礎的憑證，請在 HGS 伺服器上執行下列命令：

```powershell
$signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
$encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

Install-ADServiceAccount -Identity 'HGSgMSA'

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustHostKey
```

如果您使用本機電腦上安裝的憑證 (例如 HSM 支援的憑證和不可匯出的憑證) ，請 `-SigningCertificateThumbprint` `-EncryptionCertificateThumbprint` 改用和參數。

