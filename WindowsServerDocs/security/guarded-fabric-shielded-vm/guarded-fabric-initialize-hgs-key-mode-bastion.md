---
title: 初始化 HGS 叢集在防禦樹系中使用索引鍵模式
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9abbf85ef28ff20506558fba7dd3f5e5ee603a70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879139"
---
# <a name="initialize-the-hgs-cluster-using-key-mode-in-an-existing-bastion-forest"></a>初始化 HGS 叢集使用現有的防禦樹系中的索引鍵模式

>適用於：Windows Server 2019

>[!div class="step-by-step"]
[«在新樹系中安裝 HGS](guarded-fabric-install-hgs-in-a-bastion-forest.md)
[建立主應用程式金鑰»](guarded-fabric-create-host-key.md)

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

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustHostKey
```

如果您使用的憑證安裝在本機電腦 （例如 hsm 型憑證和非可匯出的憑證） 上，使用`-SigningCertificateThumbprint`和`-EncryptionCertificateThumbprint`參數改。

