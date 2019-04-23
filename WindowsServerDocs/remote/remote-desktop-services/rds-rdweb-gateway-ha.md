---
title: 將 RD Web 和閘道的 web 前端的高可用性
description: 提供在 RDS 部署中安裝 RD Web 和閘道伺服器的步驟。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 11/08/2016
manager: dongill
ms.openlocfilehash: fa09532e0b327b24ebb1c0e155c26c25d1043b63
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854599"
---
# <a name="add-high-availability-to-the-rd-web-and-gateway-web-front"></a>將 RD Web 和閘道的 web 前端的高可用性

>適用於：Windows Server （半年通道），Windows Server 2016


您可以部署遠端桌面 Web 存取 （RD Web 存取） 和遠端桌面閘道 （RD 閘道） 伺服器陣列來改善 Windows Server 遠端桌面服務 (RDS) 部署的延展和可用性 

將 RD Web 和閘道伺服器新增至現有的遠端桌面服務基本部署中使用下列步驟。  

## <a name="pre-requisites"></a>必要條件

設定伺服器來做為額外的 RD Web 和 RD 閘道-這可以是實體伺服器或 VM。 這包括將伺服器加入網域並啟用遠端管理。

## <a name="step-1-configure-the-new-server-to-be-part-of-the-rds-environment"></a>步驟 1：設定新的伺服器成為 RDS 環境的一部分

1. 連接到 Azure 入口網站中使用遠端桌面連線用戶端 RDMS 伺服器。
2. 將 RD Web 和閘道伺服器新增到伺服器管理員：
    1. 啟動 [伺服器管理員] 中，按一下**管理 > 新增伺服器**。   
    2. 在 [新增伺服器] 對話方塊中，按一下**立即尋找**。   
    3. 選取新建立的 RD Web 和閘道伺服器 (例如，Contoso-WebGw2)，然後按一下**確定**。
3. 加入部署中的 RD Web 和閘道伺服器  
    1. 啟動 [伺服器管理員]。  
    2. 按一下 **遠端桌面服務 > 概觀 > 部署伺服器 > 工作 > 新增 RD Web 存取伺服器**。   
    3. 選取新建立的伺服器 (例如，Contoso-WebGw2)，然後再按一下**下一步**。  
    4. 在 [確認] 頁面上選取**視需要重新啟動遠端電腦**，然後按一下**新增**。  
    5. 重複上述步驟來新增 RD 閘道伺服器，但選擇**RD 閘道伺服器**步驟 b 中。
4. 重新安裝 RD 閘道伺服器的憑證：
    1.  在 伺服器管理員 RDMS 伺服器上，按一下**遠端桌面服務 > 概觀 > 工作 > 編輯部署內容**。  
    2.  依序展開**憑證**。  
    3.  請向下捲動資料表。 按一下 RD**閘道角色服務 > 選取現有的憑證。**  
    4.  按一下 **選擇不同的憑證**，然後瀏覽至憑證位置。 例如，\Contoso-CB1\Certificates)。 選取 RD Web 和閘道伺服器的必要條件 (例如 ContosoRdGwCert) 期間建立的憑證檔案，然後按一下**開啟**。  
    5.  輸入選取的憑證的密碼**允許憑證新增至目的地電腦上的受信任的根憑證授權單位憑證存放區**，然後按一下 **確定**。  
    6.  按一下 **[套用]**。
    > [!Note] 
    > 您可能需要手動重新啟動伺服器上執行每個 RD 閘道，透過伺服器管理員] 或 [工作管理員 TSGateway 服務。
    7.  重複步驟 a 到 f 的 RD Web 存取角色服務。

## <a name="step-2-configure-rd-web-and-rd-gateway-properties-on-the-new-server"></a>步驟 2：在新的伺服器上設定 RD Web 和 RD 閘道屬性
1. 設定 RD 閘道伺服器陣列一部分的伺服器：
    1.  在 伺服器管理員 RDMS 伺服器上，按一下**所有伺服器**。 以滑鼠右鍵按一下其中一個 RD 閘道伺服器，然後**遠端桌面連線**。
    2.  登入以使用網域系統管理員帳戶的 RD 閘道伺服器。  
    3.  在 伺服器管理員在 RD 閘道伺服器上，按一下**工具 > 遠端桌面服務 > RD 閘道管理員**。  
    4.  在 導覽 窗格中，按一下 本機電腦 (例如 WebGw1 Contoso)。  
    5.  按一下 **新增的 RD 閘道伺服器伺服陣列成員**。  
    6.  在 **伺服器陣列**索引標籤上，輸入每個 RD 閘道伺服器的名稱，然後按一下 **新增**並**套用**。  
    7.  重複步驟 a 到 f 上每個 RD 閘道伺服器，使其相互辨識另一個做為 RD 閘道伺服器伺服陣列中。 不擔心如果沒有出現警告，因為它可能需要時間來傳播 DNS 設定。
2. 設定伺服器成為 RD Web 存取伺服器陣列的一部分。 下列步驟會設定兩個 RDWeb 網站相同的驗證和解密電腦金鑰。
    1.  在 伺服器管理員 RDMS 伺服器上，按一下**所有伺服器**。 以滑鼠右鍵按一下第一部 RD Web 存取伺服器 (例如 WebGw1 Contoso)，然後按一下**遠端桌面連線**。  
    2.  登入 RD Web 存取伺服器使用網域系統管理員帳戶。  
    3.  在伺服器管理員中的 RD Web 存取伺服器上，按一下**工具 > Internet Information Services (IIS) 管理員**。  
    4.  在左窗格的 IIS 管理員中，依序展開**伺服器 (例如 Contoso-WebGw1) > 站台 > Default Web Site**，然後按一下**RDWeb**。  
    5.  以滑鼠右鍵按一下**機器索引鍵**，然後按一下**開啟功能**。
    6.  在 [機器金鑰] 頁面中**動作**窗格中，選取**產生金鑰**，然後按一下**套用**。
    7.  複製驗證金鑰 (您可以以滑鼠右鍵按一下 索引鍵，然後按一下**複製**。)
    8.  在 [IIS 管理員] 中，在**Default Web Site**，選取**摘要**， **FeedLogon**並**頁面**依次。
    9. 針對每個：
        1.  以滑鼠右鍵按一下**機器索引鍵**，然後按一下**開啟功能**。
        2.  驗證金鑰，請清除**在執行階段自動產生**，然後貼上您在步驟 g 複製的索引鍵。
    10.  視窗最小化 RD 連線至此 RD Web 伺服器。  
    11.  針對第二個 RD Web 存取伺服器，結束功能檢視上重複步驟 b 到 e**機器金鑰**。
    12. 驗證金鑰，請清除**在執行階段自動產生**，然後貼上您在步驟 g 複製的索引鍵。
    13. 按一下 **[套用]**。
    14. 完成此程序**RDWeb**，**摘要**， **FeedLogon**並**頁面**頁面。
    15. 最小化到第二個 RD Web 存取伺服器中，RD 連線視窗，然後將視窗最大化 RD 連線到第一個 RD Web 存取伺服器。  
    16. 重複步驟 g，透過 n 若要複製的解密金鑰。
    17. 當驗證金鑰和解密金鑰完全相同的兩部 RD Web 存取伺服器上**RDWeb**，**摘要**， **FeedLogon**並**頁面**頁面，登出所有的 RD 連線視窗。

## <a name="step-3-configure-load-balancing-for-the-rd-web-and-rd-gateway-servers"></a>步驟 3：設定負載平衡的 RD Web 和 RD 閘道伺服器

如果您使用 Azure 基礎結構，您可以建立外部 Azure load balancer;如果沒有，您可以設定個別的硬體或軟體負載平衡器。 負載平衡是索引鍵，因此，流量會平均散發的長時間執行的連線，從遠端桌面用戶端，透過 RD 閘道，使用者將會執行其工作負載的伺服器。

> [!Note] 
> 如果您先前執行 RD Web 和 RD 閘道的伺服器已設定外部負載平衡器後方，請直接跳到步驟 4 中，選取現有的後端集區中，將新的伺服器新增至集區。

1.  建立 Azure Load Balancer:  
    1.  在 Azure 入口網站中按一下**瀏覽 > 負載平衡器 > 新增**。  
    2.  輸入名稱，例如**WebGwLB**。  
    3.  選取**公開**如**配置**，**公用 IP 位址**，以及**公用 IP 位址**。 您可以選取現有的公用 IP 位址，或建立新的。 
    4.  選取適當**訂用帳戶**，**資源群組**，並**位置**。
    5.  按一下 [建立] 。  
2. 建立[探查](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/)要監視哪些伺服器正在運作：  
    1.  在 Azure 入口網站中按一下**瀏覽 > 負載平衡器**。，負載平衡器您剛建立，例如 WebGwLB 和設定  
    2.  按一下 **探查 > 新增**。  
    3.  例如，輸入名稱， **HTTPS**，探查。 選取**TCP**做為**通訊協定**，然後輸入**443**如**連接埠**，然後按一下  **確定**。   
3.  建立 HTTPS 和 UDP 負載平衡規則：  
    1.  在 **設定**，按一下**負載平衡規則**。  
    2.  選取 **新增**for **HTTPS 規則**。  
    3.  輸入規則名稱，例如 HTTPS，然後選取**TCP** for**通訊協定**。 輸入**443**同時**連接埠**並**後端連接埠**，然後按一下 **[確定]**。  
    4.  在 **負載平衡規則**，按一下**新增**for **UDP 規則**。  
    5.  輸入規則名稱，例如**UDP**，然後選取**UDP** for**通訊協定**。 輸入**3391**同時**連接埠**並**後端連接埠**，然後按一下 **[確定]**。  
4. 建立 RD Web 和 RD 閘道伺服器的後端集區：
      1. 在 **設定**，按一下**後端位址集區 > 新增**。   
      2. 輸入名稱 (例如**WebGwBackendPool**)，然後按一下**新增虛擬機器**。  
      3. 選擇可用性設定組 (例如，「 WebGwAvSet 」)，然後按一下**確定**。   
      4. 按一下 **選擇的虛擬機器**，選取 每部虛擬機器，然後按一下**選取 > 確定 > 確定**。
