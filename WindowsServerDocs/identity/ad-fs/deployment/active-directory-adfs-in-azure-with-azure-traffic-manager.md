---
title: 與 Azure 流量管理員在 Azure 中的部署高可用性跨地區 AD FS |Microsoft Docs
description: 在這份文件中，您將學習如何在 Azure 中的 AD FS 部署高可用性。
keywords: Ad fs 搭配 Azure 流量管理員 中，使用 Azure 流量管理員地理、 多重資料中心，地理資料中心、 多重地理資料中心，adfs 部署在 azure 中的 AD FS、 部署 azure adfs，azure adfs，azure ad fs，部署 adfs，部署 ad fs，adfs，在 azure 中，部署 adfs，在 azure 中，在 azure 中，adfs azure，簡介 AD FS、 Azure、 在 Azure 中 iaas，ADFS，AD FS 部署 AD FS 將 adfs 移至 azure
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: mtillman
editor: ''
ms.assetid: a14bc870-9fad-45ed-acd5-a90ccd432e54
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: anandy;billmath
ms.openlocfilehash: 4af0386ef97694baa9ba7f1e5e7163554d79734a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874119"
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>在 Azure 與 Azure 流量管理員中的高可用性跨地區 AD FS 部署
[在 Azure 中的部署 AD FS](how-to-connect-fed-azure-adfs.md)提供有關如何部署簡單的 AD FS 基礎結構，以及在為您的組織在 Azure 中的逐步指導方針。 本文提供 Azure 中建立跨地區 AD fs 部署的後續步驟[Azure 流量管理員](https://docs.microsoft.com/azure/traffic-manager/)。 Azure 流量管理員可讓您能藉由建立地理位置 spread 高可用性和高效能 AD FS 基礎結構，為您的組織使用各種可用以符合不同的需求，從基礎結構的路由方法。

高可用性跨地區 AD FS 基礎結構可讓：

* **排除的單一失敗點：** 容錯移轉功能的 Azure 流量管理員，可以達到高可用性的 AD FS 基礎結構其中一個全球各地的組件中的資料中心中斷時即使
* **改善的效能：** 您可以使用這篇文章中的建議的部署，以提供高效能 AD FS 基礎結構，可協助使用者更快驗證。 

## <a name="design-principles"></a>設計原則
![整體設計](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

基本設計原則會列在 發行項在 Azure 中的 AD FS 部署中的設計原則與相同。 上圖顯示簡單的延伸模組的基本部署到另一個地理區域。 以下是擴充您的部署新的地理區域時，請考慮下列幾點

* **虛擬網路：** 您應該在您想要部署其他 AD FS 基礎結構的地理區域中建立新的虛擬網路。 在上圖中看到 Geo1 VNET 和 Geo2 VNET 顯示為每個地理區域中的兩個虛擬網路。
* **網域控制站和新地區 VNET 中的 AD FS 伺服器：** 建議您部署的網域控制站，在新的地理區域中，讓新的區域中的 AD FS 伺服器不需要連絡網域控制站中還有另一個網路來完成驗證，藉此改善效能。
* **儲存體帳戶：** 儲存體帳戶會與區域相關聯。 因為您要部署新的地理區域中的機器，您必須建立新的儲存體帳戶，用於在區域中。  
* **網路安全性群組：** 因為儲存體帳戶，在區域中建立的網路安全性群組不能在另一個地理區域。 因此，您必須建立新的網路安全性群組類似於 INT 和 DMZ 子網路的第一個地理區域中新的地理區域中。
* **公用 IP 位址的 DNS 標籤：** Azure 流量管理員可以參考端點只能透過 DNS 標籤。 因此，您必須建立外部負載平衡器的公用 IP 位址的 DNS 標籤。
* **Azure 流量管理員：** Microsoft Azure Traffic Manager 可讓您控制使用者流量分散到您在世界各地的不同資料中心內執行的服務端點。 Azure 流量管理員是在 DNS 層級運作。 它會使用 DNS 回應將使用者流量導向至全域散佈的端點。 用戶端然後直接連線至這些端點。 使用不同的效能、 加權和優先順序的路由選項，您可以輕鬆地選擇最適合貴組織的需求的路由選項。 
* **V net 至 V-net 兩個區域之間的連線：** 您不需要以其本身的虛擬網路之間的連線。 因為每個虛擬網路可存取網域控制站，而且 AD FS 和 WAP 伺服器本身，它們可以用沒有任何不同的區域中的虛擬網路之間的連線。 

## <a name="steps-to-integrate-azure-traffic-manager"></a>整合 Azure 流量管理員的步驟
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>部署新的地理區域中的 AD FS
請依照下列步驟和指導方針[在 Azure 中的部署 AD FS](how-to-connect-fed-azure-adfs.md)來部署新的地理區域中相同的拓撲。

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>網際網路對向 （公用） 負載平衡器的公用 IP 位址的 DNS 標籤
如先前所述，Azure 流量管理員只能將 DNS 標籤當作端點參考，因此一定要建立外部負載平衡器的公用 IP 位址的 DNS 標籤。 以下螢幕擷取畫面會顯示您可以設定的公用 IP 位址的 DNS 標籤的方式。 

![DNS 標籤](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>部署 Azure 流量管理員
請遵循下列步驟來建立流量管理員設定檔。 如需詳細資訊，您也可以參照以[管理 Azure 流量管理員設定檔](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)。

1. **建立流量管理員設定檔：** 提供您的流量管理員設定檔的唯一名稱。 此設定檔的名稱是 DNS 名稱的一部分，並做為流量管理員網域名稱標籤的前置詞。 名稱 / 前置詞會加入.trafficmanager.net，以建立 DNS 標籤，為您的流量管理員。 以下螢幕擷取畫面顯示 traffic manager 設定為 mysts 的 DNS 前置詞和產生的 DNS 標籤會是 mysts.trafficmanager.net。 
   
    ![流量管理員設定檔建立](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **流量路由方法：** 有可用在流量管理員中的三個路由選項：
   
   * Priority 
   * 效能
   * 加權
     
     **效能**是建議的選項，以達到高回應性 AD FS 基礎結構。 不過，您可以選擇任何最適合您的部署需求的路由方法。 AD FS 功能不會影響選取的路由選項。 請參閱[流量管理員流量路由方法](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)如需詳細資訊。 在此範例中可以看到螢幕擷取畫面上方**效能**所選取的方法。
3. **設定端點：** 在 [流量管理員] 頁面中，按一下端點上，選取 [新增]。 這會開啟 [新增端點] 頁面類似於以下螢幕擷取畫面
   
   ![設定端點](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   為不同的輸入，請依照下列指導方針：
   
   **類型：** 因為我們即將指向 Azure 公用 IP 位址，請選取 Azure 端點。
   
   **名稱:** 建立您想要與端點關聯的名稱。 這不是 DNS 名稱和 DNS 記錄沒有任何關係。
   
   **目標資源類型：** 您可以選取 公用 IP 位址給這個屬性的值。 
   
   **目標資源：** 這會讓您選擇不同的 DNS 標籤有訂用帳戶的 可用的選項。 選擇對應至您要設定之端點的 DNS 標籤。
   
   新增您想要 Azure 流量管理員流量路由傳送至每個地理區域的端點。
   如需詳細資訊以及有關如何新增 / 設定端點在流量管理員的詳細的步驟，請參閱[新增、 停用、 啟用或刪除端點](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints)
4. **設定探查：** 在 流量管理員 頁面中，按一下 設定。 在 [組態] 頁面中，您需要變更監視設定，以在 HTTP 連接埠 80 和相對路徑 /adfs/probe 探查
   
    ![設定探查](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **請確定端點的狀態為線上，設定完成後**。 如果所有端點都是處於 「 降級 」 狀態，Azure 流量管理員會執行盡力嘗試路由傳送流量，但前提是診斷不正確，而且所有端點都都均可。
   > 
   > 
5. **修改 DNS 記錄之 Azure 流量管理員：** 您的同盟服務應該是 Azure Traffic Manager DNS 名稱的 CNAME。 在公用 DNS 記錄中建立 CNAME，使實際人員正嘗試觸達同盟服務到 Azure 流量管理員。
   
    例如，指向將同盟服務 fs.fabidentity.com 指向流量管理員，您會需要更新 DNS 資源記錄如下：
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>測試路由和 AD FS 登入
### <a name="routing-test"></a>路由測試
路由的基本測試就是嘗試 ping 同盟服務 DNS 名稱，從每個地理區域中的機器。 根據選擇的路由方法，其實際 ping 的端點將會反映在 ping 顯示中。 比方說，如果您選取了效能路由，然後最接近的區域，用戶端會連線到端點。 以下是從兩個不同的區域用戶端電腦的兩個 ping 的快照集，一個位於 EastAsia 區域，另一個在美國西部。 

![路由測試](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>登入測試 AD FS
若要測試 AD FS 的最簡單方式是使用 IdpInitiatedSignon.aspx 網頁。 若要能夠執行，並需要啟用 AD FS 屬性上的 IdpInitiatedSignOn。 請遵循下列步驟來確認您的 AD FS 設定

1. 執行下列 cmdlet，在 AD FS 伺服器上，使用 PowerShell，將它設定為啟用。 
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
2. 從任何外部電腦存取 https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. 您應該會看到 AD FS 頁面如下所示：
   
    ![ADFS 測試-驗證挑戰](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    並在成功登入，它會提供您的成功訊息，如下所示：
   
    ![ADFS 測試-驗證成功](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>相關連結
* [在 Azure 中的基本 AD FS 部署](how-to-connect-fed-azure-adfs.md)
* [Microsoft Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/)
* [流量管理員流量路由方法](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)

## <a name="next-steps"></a>後續步驟
* [管理 Azure 流量管理員設定檔](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)
* [新增、 停用、 啟用或刪除端點](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints) 

