---
title: 將高可用性新增至 RD Web 和閘道 Web 前端
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
ms.openlocfilehash: 4e185e51b09d2e2f8ac4527f9de339de27e02f24
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "66805144"
---
# <a name="add-high-availability-to-the-rd-web-and-gateway-web-front"></a>將高可用性新增至 RD Web 和閘道 Web 前端

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016


您可以部署遠端桌面 Web 存取 (RD Web 存取) 和遠端桌面閘道 (RD 閘道) 伺服器陣列，以改善 Windows Server 遠端桌面服務 (RDS) 部署的可用性和延展性 

請使用下列步驟，將 RD Web 和閘道伺服器新增至現有的遠端桌面服務基本部署。  

## <a name="pre-requisites"></a>必要條件

設定伺服器以作為額外的 RD Web 和 RD 閘道 - 這可以是實體伺服器或 VM。 此設定包括將伺服器加入網域並啟用遠端管理。

## <a name="step-1-configure-the-new-server-to-be-part-of-the-rds-environment"></a>步驟 1：將新的伺服器設定為 RDS 環境的一部分

1. 使用遠端桌面連線用戶端，連線至 Azure 入口網站中的 RDMS 伺服器。
2. 將 RD Web 和閘道伺服器新增至伺服器管理員：
    1. 啟動 [伺服器管理員]，然後按一下 [管理 > 新增伺服器]  。   
    2. 在 [新增伺服器] 對話方塊中，按一下 [立即尋找]  。   
    3. 選取新建立的 RD Web 和閘道伺服器 (例如 Contoso-WebGw2)，然後按一下 [確定]  。
3. 將 RD Web 和閘道伺服器新增至部署  
    1. 啟動 [伺服器管理員]。  
    2. 按一下 [遠端桌面服務 > 概觀 > 部署伺服器 > 工作 > 新增 RD Web 存取伺服器]  。   
    3. 選取新建立的伺服器 (例如 Contoso-WebGw2)，然後按 [下一步]  。  
    4. 在 [確認] 頁面上選取 [視需要重新啟動遠端電腦]  ，然後按一下 [新增]  。  
    5. 重複上述步驟以新增 RD 閘道伺服器，但在步驟 b 中選擇 [RD 閘道伺服器]  。
4. 重新安裝 RD 閘道伺服器的憑證：
   1. 在 RDMS 伺服器的 [伺服器管理員] 中，按一下 [遠端桌面服務 > 概觀 > 工作 > 編輯部署內容]  。  
   2. 展開 [憑證]  。  
   3. 向下捲動至資料表。 按一下 [RD 閘道角色服務 > 選取現有的憑證]  。  
   4. 按一下 [選擇不同的憑證]  ，然後瀏覽至憑證所在位置。 例如 \Contoso-CB1\Certificates。 選取在執行必要條件檢查期間建立的 RD Web 和閘道伺服器的憑證檔案 (例如 ContosoRdGwCert)，然後按一下 [開啟]  。  
   5. 輸入憑證的密碼，並選取 [允許憑證新增至目的地電腦上受信任的根憑證授權單位憑證存放區]  ，然後按一下 [確定]  。  
   6. 按一下 [套用]  。
      > [!NOTE] 
      > 您可能需要透過伺服器管理員或工作管理員，手動重新啟動在每個 RD 閘道伺服器上執行的 TSGateway 服務。
   7. 為 RD Web 存取角色服務重複步驟 a 到 f。

## <a name="step-2-configure-rd-web-and-rd-gateway-properties-on-the-new-server"></a>步驟 2：在新的伺服器上設定 RD Web 和 RD 閘道內容
1. 將伺服器設定為 RD 閘道伺服器陣列的一部分：
    1.  在 RDMS 伺服器的 [伺服器管理員] 中，按一下 [所有伺服器]  。 以滑鼠右鍵按一下其中一個 RD 閘道伺服器，然後按一下 [遠端桌面連線]  。
    2.  使用網域系統管理員帳戶登入 RD 閘道伺服器。  
    3.  在 RD 閘道伺服器的 [伺服器管理員] 中，按一下 [工具 > 遠端桌面服務 > RD 閘道管理員]  。  
    4.  在導覽窗格中，按一下本機電腦 (例如 Contoso-WebGw1)。  
    5.  按一下 [新增 RD 閘道伺服器陣列成員]  。  
    6.  在 [伺服器陣列]  索引標籤上，輸入每個 RD 閘道伺服器的名稱，然後按一下 [新增]  和 [套用]  。  
    7.  對每個 RD 閘道伺服器重複步驟 a 到 f，使其將彼此辨識為伺服器陣列中的 RD 閘道伺服器。 若出現警告請不用擔心，因為傳播 DNS 設定可能需要一些時間。
2. 將伺服器設定為 RD Web 存取伺服器陣列的一部分。 下列步驟會將兩個 RDWeb 網站上的驗證和解密電腦金鑰設定為相同金鑰。
    1.  在 RDMS 伺服器的 [伺服器管理員] 中，按一下 [所有伺服器]  。 以滑鼠右鍵按一下第一個 RD Web 存取伺服器 (例如 Contoso-WebGw1)，然後按一下 [遠端桌面連線]  。  
    2.  使用網域系統管理員帳戶登入 RD Web 存取伺服器。  
    3.  在 RD Web 存取伺服器的 [伺服器管理員] 中，按一下 [工具 > Internet Information Services (IIS) 管理員]  。  
    4.  在 [IIS 管理員] 的左窗格中，展開 [伺服器 (例如 Contoso-WebGw1) > 網站 > 預設網站]  ，然後按一下 [RDWeb]  。  
    5.  以滑鼠右鍵按一下 [電腦金鑰]  ，然後按一下 [開啟功能]  。
    6.  在 [電腦金鑰] 頁面的 [動作]  窗格中，選取 [產生金鑰]  ，然後按一下 [套用]  。
    7.  複製驗證金鑰 (您可以用滑鼠右鍵按一下金鑰，然後按一下 [複製]  。)
    8.  在 [IIS 管理員] 中的 [預設網站]  下方，依序選取 [摘要]  、[FeedLogon]  和 [頁面]  。
    9. 針對各項：
        1.  以滑鼠右鍵按一下 [電腦金鑰]  ，然後按一下 [開啟功能]  。
        2.  針對 [驗證金鑰]，清除 [在執行階段自動產生]  ，然後貼上您在步驟 g 複製的金鑰。
    10.  將此 RD Web 伺服器的 [RD 連線] 視窗最小化。  
    11.  對第二個 RD Web 存取伺服器重複步驟 b 到 e，結束 [電腦金鑰]  的功能檢視。
    12. 針對 [驗證金鑰]，清除 [在執行階段自動產生]  ，然後貼上您在步驟 g 複製的金鑰。
    13. 按一下 [套用]  。
    14. 完成 [RDWeb]  、[摘要]  、[FeedLogon]  和 [頁面]  頁面的這項程序。
    15. 將第二個 RD Web 存取伺服器的 [RD 連線] 視窗最小化，然後將第一個 RD Web 存取伺服器的 [RD 連線] 視窗最大化。  
    16. 重複步驟 g 到 n，將解密金鑰複製過去。
    17. 在兩個 RD Web 存取伺服器的 [RDWeb]  、[摘要]  、[FeedLogon]  和 [頁面]  頁面具有相同的驗證金鑰和解密金鑰時，登出所有的 [RD 連線] 視窗。

## <a name="step-3-configure-load-balancing-for-the-rd-web-and-rd-gateway-servers"></a>步驟 3：設定 RD Web 和 RD 閘道伺服器的負載平衡

如果您使用 Azure 基礎結構，您可以建立外部 Azure Load Balancer；或者，您可以設定個別的硬體或軟體負載平衡器。 要讓流量平均分配於遠端桌面用戶端透過 RD 閘道傳至伺服器 (使用者將在此處執行其工作負載) 的長期連線，負載平衡是不可或缺的。

> [!NOTE] 
> 如果您先前執行 RD Web 和 RD 閘道的伺服器已設定於外部負載平衡器後方，請直接跳到步驟 4，選取現有的後端集區，並將新的伺服器新增至集區。

1.  建立 Azure Load Balancer：  
    1.  在 Azure 入口網站中，按一下 [瀏覽 > 負載平衡器 > 新增]  。  
    2.  輸入名稱，例如 **WebGwLB**。  
    3.  針對 [配置]  選取 [公用]  ，並選取 [公用 IP 位址]  ，然後選取一個**公用 IP 位址**。 您可以選取現有的公用 IP 位址，或建立新的位址。 
    4.  選取適當的 [訂用帳戶]  、[資源群組]  和 [位置]  。
    5.  按一下 \[建立\]  。  
2. 建立[探查](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/)以監視正在運作的伺服器：  
    1.  在 Azure 入口網站中，依序按一下 [瀏覽 > 負載平衡器]  、您剛建立的負載平衡器 (例如 WebGwLB)，以及 [設定]  
    2.  按一下 [探查 > 新增]  。  
    3.  為探查輸入名稱 (例如 **HTTPS**)。 選取 [TCP]  作為 [通訊協定]  ，並針對 [連接埠]  輸入 **443**，然後按一下 [確定]  。   
3.  建立 HTTPS 和 UDP 負載平衡規則：  
    1.  在 [設定]  中，按一下 [負載平衡規則]  。  
    2.  針對 [HTTPS 規則]  選取 [新增]  。  
    3.  輸入規則名稱 (例如 HTTPS)，然後選取 [TCP]  作為 [通訊協定]  。 針對 [連接埠]  和 [後端連接埠]  都輸入 **443**，然後按一下 [確定]  。  
    4.  在 [負載平衡規則]  中，針對 [UDP 規則]  按一下 [新增]  。  
    5.  輸入規則名稱 (例如 **UDP**)，然後選取 [UDP]  作為 [通訊協定]  。 針對 [連接埠]  和 [後端連接埠]  都輸入 **3391**，然後按一下 [確定]  。  
4. 建立 RD Web 和 RD 閘道伺服器的後端集區：
      1. 在 [設定]  中，按一下 [後端位址集區 > 新增]  。   
      2. 輸入名稱 (例如 **WebGwBackendPool**)，然後按一下 [新增虛擬機器]  。  
      3. 選擇可用性設定組 (例如 WebGwAvSet)，然後按一下 [確定]  。   
      4. 按一下 [選擇虛擬機器]  ，選取每個虛擬機器，然後按一下 [選取 > 確定 > 確定]  。
