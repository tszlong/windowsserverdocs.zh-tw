---
title: 使用防禦樹系中的金鑰模式初始化 HGS 叢集
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e72de1c85e0a9c3decf1fd3b5085363b57ca387c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403638"
---
# <a name="initialize-the-hgs-cluster-using-key-mode-in-an-existing-bastion-forest"></a>使用現有防禦樹系中的金鑰模式初始化 HGS 叢集

> 適用于： Windows Server 2019
> 
> [!div class="step-by-step"]
> [«在新樹系中安裝 HGS](guarded-fabric-install-hgs-in-a-bastion-forest.md)
> [建立主機金鑰»](guarded-fabric-create-host-key.md)

Active Directory Domain Services 將會安裝在電腦上，但應該保持未設定。

[!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)] 

在繼續之前，請確定您已預先設置主機守護者服務的叢集物件，並將 Active Directory 中的 VCO 和 CNO 物件的**完整控制權**授與已登入的使用者。
必須將虛擬電腦物件名稱傳遞至 `-HgsServiceName` 參數，並將叢集名稱傳送至 `-ClusterName` 參數。

> [!TIP]
> 再次檢查您的 AD 網域控制站，確保您的叢集物件已複寫至所有 Dc，然後再繼續。

如果您使用的是以 PFX 為基礎的憑證，請在 HGS 伺服器上執行下列命令：

```powershell
$signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
$encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

Install-ADServiceAccount -Identity 'HGSgMSA'

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustHostKey
```

如果您使用安裝在本機電腦上的憑證（例如 HSM 支援的憑證和不可匯出的憑證），請改用 `-SigningCertificateThumbprint` 並 `-EncryptionCertificateThumbprint` 參數。

