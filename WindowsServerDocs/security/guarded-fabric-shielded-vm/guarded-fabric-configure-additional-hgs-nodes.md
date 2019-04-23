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
ms.openlocfilehash: a89337457cc71ffee78e3f73fecc2262f1fb38e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855199"
---
# <a name="configure-additional-hgs-nodes"></a>設定其他 HGS 節點

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

在生產環境中 HGS 應該設定高可用性叢集中以確保，電源開啟的受防護的 Vm 即使 HGS 節點發生故障。 針對測試環境，就不需要次要的 HGS 節點。

您可以使用其中一種方法來新增為最適合您環境的 HGS 節點。

|                |                         |                              | 
|----------------|-------------------------|------------------------------|
|新的 HGS 樹系  | [使用 PFX 檔案](#dedicated-hgs-forest-with-pfx-certificates) | [使用憑證指紋](#dedicated-hgs-forest-with-certificate-thumbprints) |
|現有的防禦樹系 |  [使用 PFX 檔案](#existing-bastion-forest-with-pfx-certificates) | [使用憑證指紋](#existing-bastion-forest-with-certificate-thumbprints) |

## <a name="prerequisites"></a>先決條件

請確定每個額外的節點： 
- 具有相同的硬體和軟體設定為主要節點 
- 連接到其他 HGS 伺服器相同的網路
- 可以由其 DNS 名稱解析其他 HGS 伺服器

## <a name="dedicated-hgs-forest-with-pfx-certificates"></a>使用 PFX 憑證的專用的 HGS 樹系

1. HGS 將節點升級到網域控制站
2. HGS 伺服器初始化

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>HGS 將節點升級到網域控制站

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>HGS 伺服器初始化

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="dedicated-hgs-forest-with-certificate-thumbprints"></a>使用憑證指紋的專用的 HGS 樹系
 
1. HGS 將節點升級到網域控制站
2. HGS 伺服器初始化
3. 安裝憑證的私密金鑰

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>HGS 將節點升級到網域控制站

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>HGS 伺服器初始化

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

### <a name="install-the-private-keys-for-the-certificates"></a>安裝憑證的私密金鑰

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="existing-bastion-forest-with-pfx-certificates"></a>使用 PFX 憑證的現有防禦樹系

1. 將節點加入現有的網域
2. 擷取 gMSA 密碼，並執行 Install-adserviceaccount 機器權限授與
3. HGS 伺服器初始化

### <a name="join-the-node-to-the-existing-domain"></a>將節點加入現有的網域

1. 請確定至少一個 NIC 在節點上的已設定為使用您的第一部 HGS 伺服器的 DNS 伺服器。
2. 將新的 HGS 節點加入您的第一個 HGS 節點相同的網域中。 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>擷取 gMSA 密碼，並執行 Install-adserviceaccount 機器權限授與

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>HGS 伺服器初始化

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="existing-bastion-forest-with-certificate-thumbprints"></a>使用憑證指紋的現有防禦樹系

1. 將節點加入現有的網域
2. 擷取 gMSA 密碼，並執行 Install-adserviceaccount 機器權限授與
3. HGS 伺服器初始化
4. 安裝憑證的私密金鑰

### <a name="join-the-node-to-the-existing-domain"></a>將節點加入現有的網域

1. 請確定至少一個 NIC 在節點上的已設定為使用您的第一部 HGS 伺服器的 DNS 伺服器。
2. 將新的 HGS 節點加入您的第一個 HGS 節點相同的網域中。 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>擷取 gMSA 密碼，並執行 Install-adserviceaccount 機器權限授與

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>HGS 伺服器初始化

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

從第一部 HGS 伺服器複寫到這個節點需要 10 分鐘的時間來加密和簽署憑證。

### <a name="install-the-private-keys-for-the-certificates"></a>安裝憑證的私密金鑰

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="configure-hgs-for-https-communications"></a>將 HGS 設定為 HTTPS 通訊

如果您想要保護 HGS 使用 SSL 憑證的端點，您就必須在此節點，以及 HGS 叢集中的每個節點上設定 SSL 憑證。
SSL 憑證*不是*由 HGS 複寫並不需要使用相同的金鑰，每個節點 （也就是您可以有不同的 SSL 憑證，每個節點）。

當要求的 SSL 憑證，請確定叢集的完整的網域名稱 (如所示的輸出中`Get-HgsServer`) 是其中一個，主體一般名稱的憑證，或納入為主體替代 DNS 名稱。
當您已從您的憑證授權單位取得憑證時，您可以設定與 HGS[組 HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/set-hgsserver)。

```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

如果您已安裝憑證的本機憑證存放區，並想要參考的憑證指紋，請改為執行下列命令：

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

HGS 一律會公開 HTTP 和 HTTPS 連接埠進行通訊。
不支援移除 HTTP 繫結在 IIS 中，不過您可以使用 Windows 防火牆或其他網路防火牆技術來透過連接埠 80 會封鎖通訊。

## <a name="decommission-an-hgs-node"></a>解除委任的 HGS 節點

若要解除委任的 HGS 節點：

1. [清除 HGS 設定](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration)。

   這會從叢集移除節點，並證明與金鑰保護服務會解除安裝。 
   如果它是在叢集中的最後一個節點、-Force，才能代表您要移除的最後一個節點，然後終結 Active Directory 中的叢集。 
   
   如果 HGS 部署在防禦樹系中 （預設值），這會是唯一的步驟。 
   您可以選擇性地將電腦從網域退出，並移除 Active Directory 中 gMSA 帳戶。

1. 如果 HGS 建立自己的網域，您也應該[解除安裝 HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration)退出網域和網域控制站降級。



## <a name="next-step"></a>後續步驟

>[!div class="nextstepaction"]
[驗證 HGS 設定](guarded-fabric-verify-hgs-configuration.md)

