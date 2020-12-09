---
title: 設定其他 HGS 節點
ms.topic: article
ms.assetid: 227f723b-acb2-42a7-bbe3-44e82f930e35
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 01/14/2020
ms.openlocfilehash: 97e3860d96fe87414fba9d4965bfde62208c01be
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96864817"
---
# <a name="configure-additional-hgs-nodes"></a>設定其他 HGS 節點

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

在生產環境中，必須在高可用性叢集中設定 HGS，以確保即使 HGS 節點停止運作，受防護的 Vm 仍能開啟電源。 針對測試環境，不需要次要 HGS 節點。

使用其中一種方法來新增 HGS 節點，最適合您的環境。

| 環境 | 選項 1 | 選項 2 |
|--|--|--|
| 新的 HGS 樹系 | [使用 PFX 檔案](#dedicated-hgs-forest-with-pfx-certificates) | [使用憑證指紋](#dedicated-hgs-forest-with-certificate-thumbprints) |
| 現有的防禦樹系 | [使用 PFX 檔案](#existing-bastion-forest-with-pfx-certificates) | [使用憑證指紋](#existing-bastion-forest-with-certificate-thumbprints) |

## <a name="prerequisites"></a>必要條件

請確定每個額外的節點：
- 具有與主要節點相同的硬體和軟體設定
- 連接到與其他 HGS 伺服器相同的網路
- 可以透過 DNS 名稱來解析其他 HGS 伺服器

## <a name="dedicated-hgs-forest-with-pfx-certificates"></a>具有 PFX 憑證的專用 HGS 樹系

1. 將 HGS 節點升階至網域控制站
2. 初始化 HGS 伺服器

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>將 HGS 節點升階至網域控制站

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)]

### <a name="initialize-the-hgs-server"></a>初始化 HGS 伺服器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)]

## <a name="dedicated-hgs-forest-with-certificate-thumbprints"></a>具有憑證指紋的專用 HGS 樹系

1. 將 HGS 節點升階至網域控制站
2. 初始化 HGS 伺服器
3. 安裝憑證的私密金鑰

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>將 HGS 節點升階至網域控制站

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)]

### <a name="initialize-the-hgs-server"></a>初始化 HGS 伺服器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)]

### <a name="install-the-private-keys-for-the-certificates"></a>安裝憑證的私密金鑰

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="existing-bastion-forest-with-pfx-certificates"></a>具有 PFX 憑證的現有防禦樹系

1. 將節點加入現有的網域
2. 授與機器許可權，以取出 gMSA 密碼並執行 Install-ADServiceAccount
3. 初始化 HGS 伺服器

### <a name="join-the-node-to-the-existing-domain"></a>將節點加入現有的網域

1. 確定節點上至少有一個 NIC 已設定為在您的第一部 HGS 伺服器上使用 DNS 伺服器。
2. 將新的 HGS 節點加入至與您的第一個 HGS 節點相同的網域。

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>授與機器許可權，以取出 gMSA 密碼並執行 Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)]

### <a name="initialize-the-hgs-server"></a>初始化 HGS 伺服器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)]

## <a name="existing-bastion-forest-with-certificate-thumbprints"></a>具有憑證指紋的現有防禦樹系

1. 將節點加入現有的網域
2. 授與機器許可權，以取出 gMSA 密碼並執行 Install-ADServiceAccount
3. 初始化 HGS 伺服器
4. 安裝憑證的私密金鑰

### <a name="join-the-node-to-the-existing-domain"></a>將節點加入現有的網域

1. 確定節點上至少有一個 NIC 已設定為在您的第一部 HGS 伺服器上使用 DNS 伺服器。
2. 將新的 HGS 節點加入至與您的第一個 HGS 節點相同的網域。

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>授與機器許可權，以取出 gMSA 密碼並執行 Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)]

### <a name="initialize-the-hgs-server"></a>初始化 HGS 伺服器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)]

最多需要10分鐘的時間，才會將第一部 HGS 伺服器的加密和簽署憑證複寫到這個節點。

### <a name="install-the-private-keys-for-the-certificates"></a>安裝憑證的私密金鑰

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="configure-hgs-for-https-communications"></a>設定 HGS 以進行 HTTPS 通訊

如果您想要使用 SSL 憑證來保護 HGS 端點，您必須在此節點上設定 SSL 憑證，以及 HGS 叢集中的每個其他節點。
HGS *不會* 複寫 SSL 憑證，而且每個節點都不需要使用相同的金鑰 (也就是每個節點) 都可以有不同的 ssl 憑證。

要求 SSL 憑證時，請確定叢集的完整功能變數名稱 (如) 的輸出中所示，這是憑證的 `Get-HgsServer` 主體一般名稱，或包含為主體替代 DNS 名稱。
當您從憑證授權單位單位取得憑證時，您可以設定 HGS 將它與 [HgsServer](/powershell/module/hgsserver/set-hgsserver)搭配使用。

```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

如果您已將憑證安裝至本機憑證存放區，並想要依指紋參考該憑證，請改為執行下列命令：

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

HGS 一律會公開 HTTP 和 HTTPS 埠以進行通訊。
不支援在 IIS 中移除 HTTP 系結，不過您可以使用 Windows 防火牆或其他網路防火牆技術來封鎖埠80的通訊。

## <a name="decommission-an-hgs-node"></a>解除委任 HGS 節點

若要解除委任 HGS 節點：

1. [清除 [HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration)設定]。

   這會從叢集中移除節點，並卸載證明和金鑰保護服務。
   如果是叢集中的最後一個節點，則需要-Force，以表示您想要移除最後一個節點，並在 Active Directory 中終結叢集。

   如果在防禦樹系中部署 HGS (預設) ，那就是唯一的步驟。
   您可以選擇性地將電腦從網域中移除，並從 Active Directory 移除 gMSA 帳戶。

2. 如果 HGS 建立了自己的網域，您也應該 [卸載 HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration) 來退出網域，並將網域控制站降級。
