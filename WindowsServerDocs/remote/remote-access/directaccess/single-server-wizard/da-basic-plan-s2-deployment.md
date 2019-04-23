---
title: 步驟 2 規劃基本 DirectAccess 部署
description: 本主題是指南部署單一 DirectAccess 伺服器使用取得啟動精靈的 Windows Server 2016 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ddcb162-dd92-406c-acab-d3de7239c644
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8c21b7fa62246170caeb07cb5865c1ff311e0f09
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848749"
---
# <a name="step-2-plan-the-basic-directaccess-deployment"></a>步驟 2 規劃基本 DirectAccess 部署

>適用於：Windows Server （半年通道），Windows Server 2016

規劃 DirectAccess 基礎結構之後, 使用基本設定在單一伺服器上部署 DirectAccess 的下一步是規劃開始使用精靈的設定。  
  
|工作|描述|  
|----|--------|  
|規劃用戶端部署|根據預設，開始使用精靈將 DirectAccess 部署到所有膝上型電腦和網域中的 notebook 電腦將 WMI 篩選器套用至用戶端設定 GPO|  
|DirectAccess 伺服器部署規劃|規劃如何部署 DirectAccess 伺服器。|  
  
## <a name="bkmk_2_1_client"></a>用戶端部署規劃  
規劃用戶端部署時要進行兩個決策：  
  
1.  DirectAccess 將僅供攜帶型電腦使用，或是可供任何電腦使用？  
  
    當您在 開始使用精靈設定 DirectAccess 用戶端時，您可以選擇只允許行動電腦使用 DirectAccess 連線指定的安全性群組中。 如果您將存取限於攜帶型電腦時，DirectAccess 就會自動設定 WMI 篩選器，以確保 DirectAccess 用戶端 GPO 只會套用到指定之安全性群組中的攜帶型電腦。 DirectAccess 系統管理員需要建立或修改群組原則 WMI 篩選器，來啟用此設定的權限。  
  
2.  哪些安全性群組將包含 DirectAccess 用戶端電腦？  
  
    DirectAccess 設定包含在 DirectAccess 用戶端 GPO 中。 GPO 會套用到屬於您在開始使用精靈中指定之安全性群組的電腦。 您可以指定在任何支援的網域中所包含的安全性群組。 設定 DirectAccess 之前，應該建立的安全性群組。 您可以完成 DirectAccess 部署中之後，將電腦新增到安全性群組，但請注意，是否您新增至安全性群組的不同網域中的用戶端電腦時，用戶端 GPO 將不會套用到這些用戶端。 例如，如果您在網域 A 中建立 SG1 做為 DirectAccess 用戶端，之後新增網域 B 的用戶端到此群組，用戶端 GPO 將不會套用到網域 B 中的用戶端。若要避免這個問題，請為每個包含用戶端電腦的網域建立新的用戶端安全性群組。 或者，如果您不想要建立新的安全性群組，請執行 Add-DAClient Cmdlet，加上新網域的新 GPO 名稱。  
  
## <a name="bkmk_2_2_server"></a>DirectAccess 伺服器部署規劃  
有許多的計劃部署 DirectAccess 伺服器時所做的決策：  
  
-   **網路拓撲**-有可用的兩個拓撲部署 DirectAccess 伺服器時：  
  
    -   **兩張介面卡**-可以使用一個網路介面卡直接連線到網際網路，設定 DirectAccess 使用兩張網路介面卡，而且其他連接到內部網路。 或者，伺服器安裝在邊緣裝置 (例如防火牆或路由器) 後面。 在此設定中，一張網路介面卡連線到周邊網路，另一張連線到內部網路。  
  
    -   **單一網路介面卡**-在此設定 DirectAccess 伺服器安裝在邊緣裝置，例如防火牆或路由器後面。 網路介面卡會連線到內部網路。  
  
-   **網路介面卡**-DirectAccess 精靈會自動偵測設定 DirectAccess 伺服器上的網路介面卡。 您可以確定會從選取正確的介面卡**檢閱**頁面。  
  
-   **IP-HTTPS 憑證**-因為未在此部署所需的 PKI 時，精靈會自動為 IP-HTTPS 和網路位置伺服器 （如果不有任何憑證），佈建自我簽署的憑證，並會自動啟用Kerberos proxy。 精靈也會啟用 NAT64 和 DNS64 僅支援 IPv4 的環境中的通訊協定轉譯。 精靈套用設定成功完成之後，按一下 [關閉]。  
  
-   **Windows 7 用戶端**-您無法啟用 Windows 7 用戶端，從 開始使用精靈的支援。 這可啟用從 進階安裝精靈。 如需詳細資訊，請參閱 <<c0> [ 部署單一 DirectAccess 伺服器使用進階設定](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。  
  
-   **VPN 設定**-設定 DirectAccess 之前，請先決定您是否要提供 VPN 存取給遠端用戶端。 如果您有貴組織中不支援 DirectAccess 連線的用戶端電腦，您應該提供 VPN 存取權 （可能是因為它們未受管理或執行不支援 DirectAccess 的作業系統）。 開始使用精靈設定使用 DHCP 的 VPN IP 位址指派，並設定 VPN 用戶端以使用 Active Directory 進行驗證。  
  
-   **強制通道**-如果您打算使用強制通道，或可能在未來新增它，您應該使用[部署單一 DirectAccess 伺服器使用進階設定](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)部署雙通道設定。 基於安全考量，不支援強制通道，在單一通道設定。  
  
## <a name="BKMK_Links"></a>上一個步驟  
  
-   [步驟 1：規劃基本 DirectAccess 基礎結構](da-basic-plan-s1-infrastructure.md)  
  


