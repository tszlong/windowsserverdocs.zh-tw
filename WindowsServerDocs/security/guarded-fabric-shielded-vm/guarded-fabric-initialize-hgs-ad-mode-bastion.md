---
title: 在防禦樹系中使用 AD 模式初始化 HGS 叢集
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 0e2845153ce2cf6d21deb02eab0e6518c5a6b351
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946361"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-an-existing-bastion-forest"></a>在現有的防禦樹系中使用 AD 模式初始化 HGS 叢集

>適用於：Windows Server (半年度管道)、Windows Server 2016


>[!IMPORTANT]
>從 Windows Server 2019 開始，系統管理員信任的證明 (AD 模式) 已淘汰。 針對不可能進行 TPM 證明的環境，請設定[主機金鑰證明](guarded-fabric-initialize-hgs-key-mode-bastion.md)。 主機金鑰證明提供與 AD 模式類似的保證，而且設定起來較簡單。

Active Directory Domain Services 將會安裝在電腦上，但應該保持未設定。

[!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

在繼續之前，請確定您已預先設置主機守護者服務的叢集物件，並將 Active Directory 中的 VCO 和 CNO 物件的**完整控制權**授與已登入的使用者。
虛擬電腦物件名稱必須傳遞給 `-HgsServiceName` 參數，並將叢集名稱傳遞給 `-ClusterName` 參數。

> [!TIP]
> 再次檢查您的 AD 網域控制站，確保您的叢集物件已複寫至所有 Dc，然後再繼續。

如果您使用的是以 PFX 為基礎的憑證，請在 HGS 伺服器上執行下列命令：

```powershell
$signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
$encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

Install-ADServiceAccount -Identity 'HGSgMSA'

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
```

如果您使用安裝在本機電腦上的憑證 (例如 HSM 支援的憑證和不可匯出的憑證) ，請改用 `-SigningCertificateThumbprint` 和 `-EncryptionCertificateThumbprint` 參數。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [設定網狀架構 DNS](guarded-fabric-configuring-fabric-dns-ad.md)

