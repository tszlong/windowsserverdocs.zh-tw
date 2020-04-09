---
title: 使用 Azure 流量管理員在 Azure 中部署高可用性跨地理位置 AD FS |Microsoft Docs
description: 如何在 Azure 中部署 AD FS 以獲得高可用性。
services: active-directory
author: anandyadavmsft
manager: mtillman
ms.prod: windows-server
ms.assetid: a14bc870-9fad-45ed-acd5-a90ccd432e54
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: anandy;billmath
ms.openlocfilehash: 9bfb59fadd2cf6b07d3c47ab69f0fe67974706a3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855201"
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>使用 Azure 流量管理員在 Azure 中部署高可用性跨地理位置 AD FS
[Azure 中的 AD FS 部署](how-to-connect-fed-azure-adfs.md)提供逐步指導方針，說明如何在 azure 中為您的組織部署簡單的 AD FS 基礎結構。 本文提供使用[azure 流量管理員](https://docs.microsoft.com/azure/traffic-manager/)在 azure 中建立跨地理位置部署 AD FS 的後續步驟。 Azure 流量管理員藉由使用各種可用的路由方法，以符合基礎結構的不同需求，協助為您的組織建立散佈高可用性和高效能 AD FS 基礎結構的地理位置。

高可用性跨地理位置 AD FS 基礎結構可讓：

* **消除單一失敗點：** 有了 Azure 流量管理員的容錯移轉功能，即使在地球中的其中一個資料中心停止運作時，您仍可達到高可用性的 AD FS 基礎結構
* **改善的效能：** 您可以使用本文中建議的部署，提供高效能 AD FS 的基礎結構，以協助使用者更快速地進行驗證。 

## <a name="design-principles"></a>設計原則
![整體設計](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

基本的設計原則會與在 Azure 中的 AD FS 部署一文中的設計原則中所列的相同。 上圖顯示基本部署至另一個地理區域的簡單擴充。 以下是將您的部署擴充到新的地理區域時要考慮的一些重點

* **虛擬網路：** 您應該在想要部署額外 AD FS 基礎結構的地理區域中建立新的虛擬網路。 在上圖中，您會看到 Geo1 VNET 和 Geo2 VNET 做為每個地理區域中的兩個虛擬網路。
* **新的地理 VNET 中的網域控制站和 AD FS 伺服器：** 建議您在新的地理區域中部署網域控制站，讓新區域中的 AD FS 伺服器不需要連線到其他遠距離網路中的網域控制站來完成驗證，因而改善效能。
* **儲存體帳戶：** 儲存體帳戶會與某個區域相關聯。 因為您將在新的地理區域中部署機器，所以您必須建立要在區域中使用的新儲存體帳戶。  
* **網路安全性群組：** 作為儲存體帳戶，在區域中建立的網路安全性群組不能用於另一個地理區域。 因此，您必須建立新的網路安全性群組，類似于新地理區域中 INT 和 DMZ 子網的第一個地理區域。
* **公用 IP 位址的 DNS 標籤：** Azure 流量管理員只能透過 DNS 標籤參考端點。 因此，您必須為外部負載平衡器的公用 IP 位址建立 DNS 標籤。
* **Azure 流量管理員：** Microsoft Azure 流量管理員可讓您控制如何將使用者流量分散到您的服務端點（在世界各地的不同資料中心內執行）。 Azure 流量管理員在 DNS 層級運作。 它會使用 DNS 回應，將使用者流量導向至全域散發的端點。 接著，用戶端會直接連接到這些端點。 透過不同的效能、加權和優先順序路由選項，您可以輕鬆地選擇最適合您組織需求的路由選項。 
* **在兩個區域之間的 v-net 到 v-net 連線能力：** 您不需要具備虛擬網路本身之間的連線能力。 由於每個虛擬網路都可存取網域控制站，而且具有 AD FS 和 WAP 伺服器本身，因此在不同區域中的虛擬網路之間不會有任何連線能力。 

## <a name="steps-to-integrate-azure-traffic-manager"></a>整合 Azure 流量管理員的步驟
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>在新的地理區域中部署 AD FS
請遵循在[Azure 中 AD FS 部署](how-to-connect-fed-azure-adfs.md)中的步驟和指導方針，在新的地理區域中部署相同的拓撲。

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>網際網路對向（公用）負載平衡器公用 IP 位址的 DNS 標籤
如先前所述，Azure 流量管理員只能將 DNS 標籤稱為端點，因此請務必為外部負載平衡器的公用 IP 位址建立 DNS 標籤。 下列螢幕擷取畫面顯示如何設定公用 IP 位址的 DNS 標籤。 

![DNS 標籤](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>部署 Azure 流量管理員
請遵循下列步驟來建立流量管理員設定檔。 如需詳細資訊，您也可以參考[管理 Azure 流量管理員設定檔](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)。

1. **建立流量管理員設定檔：** 為您的流量管理員設定檔提供一個唯一的名稱。 此設定檔的名稱是 DNS 名稱的一部分，並作為流量管理員功能變數名稱標籤的前置詞。 名稱/前置詞會新增至. trafficmanager.net，以建立流量管理員的 DNS 標籤。 下列螢幕擷取畫面顯示將流量管理員 DNS 首碼設定為 mysts，並將會 mysts.trafficmanager.net 產生的 DNS 標籤。 
   
    ![建立流量管理員設定檔](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **流量路由方法：** 流量管理員中有三個可用的路由選項：
   
   * 優先順序 
   * 效能
   * 加權
     
     **效能**是達到高回應性 AD FS 基礎結構的建議選項。 不過，您可以選擇最適合您的部署需求的任何路由方法。 選取的路由選項不會影響 AD FS 功能。 如需詳細資訊，請參閱[流量管理員流量路由方法](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)。 在上面的範例螢幕擷取畫面中，您可以看到已選取**效能**方法。
3. **設定端點：** 在 [流量管理員] 頁面上，按一下 [端點]，然後選取 [新增]。 這會開啟類似下列螢幕擷取畫面的 [新增端點] 頁面
   
   ![設定端點](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   針對不同的輸入，請遵循下列指導方針：
   
   **類型：** 選取 Azure 端點，因為我們會指向 Azure 公用 IP 位址。
   
   **名稱：** 建立您想要與端點建立關聯的名稱。 這不是 DNS 名稱，而且與 DNS 記錄無關。
   
   **目標資源類型：** 選取 [公用 IP 位址] 做為此屬性的值。 
   
   **目標資源：** 這可讓您選擇您的訂用帳戶所提供的不同 DNS 標籤。 選擇對應至您要設定之端點的 [DNS] 標籤。
   
   針對您想要 Azure 流量管理員將流量路由傳送到的每個地理區域新增端點。
   如需有關如何在流量管理員中新增/設定端點的詳細資訊和詳細步驟，請參閱[新增、停用、啟用或刪除端點](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints)
4. **設定探查：** 在 [流量管理員] 頁面中，按一下 [設定]。 在 [設定] 頁面中，您需要將監視設定變更為在 HTTP 埠80和相對路徑/adfs/probe 的探查
   
    ![設定探查](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **完成設定之後，請確定端點的狀態為線上**。 如果所有端點都處於「已降級」狀態，Azure 流量管理員將會盡力路由傳送流量，假設診斷不正確，且所有端點都可連線。
   > 
   > 
5. **修改 Azure 流量管理員的 DNS 記錄：** 您的同盟服務應該是 Azure 流量管理員 DNS 名稱的 CNAME。 在公用 DNS 記錄中建立 CNAME，讓嘗試連線到 federation service 的人實際達到 Azure 流量管理員。
   
    例如，若要將 federation service fs.fabidentity.com 指向流量管理員，您需要更新 DNS 資源記錄，如下所示：
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>測試路由和 AD FS 登入
### <a name="routing-test"></a>路由測試
路由的一項非常基本的測試，就是嘗試從每個地理區域中的電腦 ping federation service DNS 名稱。 根據選擇的路由方法，其實際 ping 的端點會反映在 ping 顯示中。 例如，如果您選取了效能路由，則會到達最接近用戶端區域的端點。 以下是來自兩個不同區域用戶端電腦的兩個 ping 的快照，一個在 EastAsia 區域，另一個在美國西部。 

![路由測試](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>AD FS 登入測試
測試 AD FS 最簡單的方式是使用 IDPInitiatedsignon.aspx) .aspx 頁面。 為了能夠這樣做，必須在 AD FS 屬性上啟用 IDPInitiatedsignon.aspx)。 請遵循下列步驟來確認您的 AD FS 設定

1. 使用 PowerShell 在 AD FS 伺服器上執行下列 Cmdlet，將其設定為 [已啟用]。 
   Set-adfsproperties-EnableIdPInitiatedSignonPage $true
2. 從任何外部電腦存取 HTTPs://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. 您應該會看到如下所示的 [AD FS] 頁面：
   
    ![ADFS 測試-驗證挑戰](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    成功登入時，它會提供您成功的訊息，如下所示：
   
    ![ADFS 測試-驗證成功](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>相關連結
* [Azure 中的基本 AD FS 部署](how-to-connect-fed-azure-adfs.md)
* [Microsoft Azure 流量管理員](https://docs.microsoft.com/azure/traffic-manager/)
* [流量管理員流量路由方法](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)

## <a name="next-steps"></a>後續步驟
* [管理 Azure 流量管理員設定檔](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)
* [新增、停用、啟用或刪除端點](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints) 

