---
title: 設定其他 HGS 節點
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 227f723b-acb2-42a7-bbe3-44e82f930e35
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 10/22/2018
ms.openlocfilehash: fe9cd63f08da849c2cb002ab853501370d736f0f
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870594"
---
# <a name="configure-additional-hgs-nodes"></a>設定其他 HGS 節點

>適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

在生產環境中，您應該在高可用性叢集中設定 HGS，以確保即使 HGS 節點停止運作，受防護的 Vm 仍可開機。 針對測試環境，不需要次要 HGS 節點。

使用其中一種方法來新增 HGS 節點，最適合您的環境。

|                |                         |                              | 
|----------------|-------------------------|------------------------------|
|新的 HGS 樹系  | [使用 PFX 檔案](#dedicated-hgs-forest-with-pfx-certificates) | [使用憑證指紋](#dedicated-hgs-forest-with-certificate-thumbprints) |
|現有的防禦樹系 |  [使用 PFX 檔案](#existing-bastion-forest-with-pfx-certificates) | [使用憑證指紋](#existing-bastion-forest-with-certificate-thumbprints) |

## <a name="prerequisites"></a>必要條件

請確定每個額外的節點： 
- 具有與主要節點相同的硬體和軟體設定 
- 連線到與其他 HGS 伺服器相同的網路
- 可以透過其 DNS 名稱來解析其他 HGS 伺服器

## <a name="dedicated-hgs-forest-with-pfx-certificates"></a>具有 PFX 憑證的專用 HGS 樹系

1. 將 HGS 節點升級至網域控制站
2. 初始化 HGS 伺服器

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>將 HGS 節點升級至網域控制站

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>初始化 HGS 伺服器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="dedicated-hgs-forest-with-certificate-thumbprints"></a>具有憑證指紋的專用 HGS 樹系
 
1. 將 HGS 節點升級至網域控制站
2. 初始化 HGS 伺服器
3. 安裝憑證的私密金鑰

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>將 HGS 節點升級至網域控制站

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>初始化 HGS 伺服器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

### <a name="install-the-private-keys-for-the-certificates"></a>安裝憑證的私密金鑰

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="existing-bastion-forest-with-pfx-certificates"></a>具有 PFX 憑證的現有防禦樹系

1. 將節點加入現有的網域
2. 授與機器許可權以取得 gMSA 密碼並執行安裝-Uninstall-adserviceaccount
3. 初始化 HGS 伺服器

### <a name="join-the-node-to-the-existing-domain"></a>將節點加入現有的網域

1. 請確定節點上至少有一個 NIC 設定為使用第一部 HGS 伺服器上的 DNS 伺服器。
2. 將新的 HGS 節點加入與您的第一個 HGS 節點相同的網域。 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>授與機器許可權以取得 gMSA 密碼並執行安裝-Uninstall-adserviceaccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>初始化 HGS 伺服器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="existing-bastion-forest-with-certificate-thumbprints"></a>具有憑證指紋的現有防禦樹系

1. 將節點加入現有的網域
2. 授與機器許可權以取得 gMSA 密碼並執行安裝-Uninstall-adserviceaccount
3. 初始化 HGS 伺服器
4. 安裝憑證的私密金鑰

### <a name="join-the-node-to-the-existing-domain"></a>將節點加入現有的網域

1. 請確定節點上至少有一個 NIC 設定為使用第一部 HGS 伺服器上的 DNS 伺服器。
2. 將新的 HGS 節點加入與您的第一個 HGS 節點相同的網域。 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>授與機器許可權以取得 gMSA 密碼並執行安裝-Uninstall-adserviceaccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>初始化 HGS 伺服器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

最多需要10分鐘的時間，來自第一部 HGS 伺服器的加密和簽署憑證才會複寫到此節點。

### <a name="install-the-private-keys-for-the-certificates"></a>安裝憑證的私密金鑰

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="configure-hgs-for-https-communications"></a>設定 HGS 進行 HTTPS 通訊

如果您想要使用 SSL 憑證來保護 HGS 端點，您必須在此節點上設定 SSL 憑證，以及在 HGS 叢集中的每個其他節點。
HGS*不會*複寫 SSL 憑證，而且不需要針對每個節點使用相同的金鑰（也就是每個節點可以有不同的 SSL 憑證）。

要求 SSL 憑證時，請確定叢集的完整功能變數名稱（如的輸出`Get-HgsServer`中所示）是憑證的主體一般名稱，或包含為主體替代 DNS 名稱。
當您從憑證授權單位單位取得憑證時，您可以設定 HGS 將它與[HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/set-hgsserver)搭配使用。

```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

如果您已經將憑證安裝到本機憑證存放區，而且想要依指紋參考它，請改為執行下列命令：

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

HGS 一律會公開用於通訊的 HTTP 和 HTTPS 埠。
不支援在 IIS 中移除 HTTP 系結，不過您可以使用 Windows 防火牆或其他網路防火牆技術來封鎖透過埠80的通訊。

## <a name="decommission-an-hgs-node"></a>解除委任 HGS 節點

解除委任 HGS 節點：

1. [清除 HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration)設定。

   這會從叢集中移除節點，並卸載證明和金鑰保護服務。 
   如果它是叢集中的最後一個節點，則需要-Force，以表示您想要移除最後一個節點，並在 Active Directory 中摧毀叢集。 
   
   如果在防禦樹系（預設值）中部署 HGS，這就是唯一的步驟。 
   您可以選擇性地將電腦從網域退出，並從 Active Directory 移除 gMSA 帳戶。

1. 如果 HGS 建立了自己的網域，您也應該[卸載 hgs](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration)以退出網域，並將網域控制站降級。



## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [驗證 HGS 設定](guarded-fabric-verify-hgs-configuration.md)

