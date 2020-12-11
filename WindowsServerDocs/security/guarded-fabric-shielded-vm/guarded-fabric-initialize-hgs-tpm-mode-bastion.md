---
description: 深入瞭解：在現有的防禦樹系中使用 TPM 模式初始化 HGS 叢集
title: 在防禦樹系中使用 TPM 模式初始化 HGS 叢集
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: de96fa3a0ad8ce4b76bd4b3c0d484bc448906ae6
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049746"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-an-existing-bastion-forest"></a>在現有的防禦樹系中使用 TPM 模式初始化 HGS 叢集

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

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

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
```

如果您使用本機電腦上安裝的憑證 (例如 HSM 支援的憑證和不可匯出的憑證) ，請 `-SigningCertificateThumbprint` `-EncryptionCertificateThumbprint` 改用和參數。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [安裝 TPM 根憑證](guarded-fabric-install-trusted-tpm-root-certificates.md)
