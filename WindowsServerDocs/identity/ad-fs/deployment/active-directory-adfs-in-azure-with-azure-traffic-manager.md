---
title: 使用 Azure 流量管理員在 Azure 中部署高可用性跨地區 AD FS | Microsoft Docs
description: 如何在 Azure 中部署 AD FS 以獲得高可用性。
services: active-directory
author: anandyadavmsft
manager: mtillman
ms.assetid: a14bc870-9fad-45ed-acd5-a90ccd432e54
ms.topic: how-to
ms.date: 09/01/2016
ms.author: billmath
ms.openlocfilehash: fafeffdc44af7dd59501355f6d40d46264bbf1af
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949584"
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>使用 Azure 流量管理員在 Azure 中部署高可用性跨地區 AD FS
[Azure 中的 AD FS 部署](how-to-connect-fed-azure-adfs.md) 提供有關如何在 Azure 中為您的組織部署簡單 AD FS 基礎結構的逐步指導方針。 本文會提供後續的步驟，以使用 [Azure 流量管理員](/azure/traffic-manager/)在 Azure 中建立跨地區的 AD FS 部署。 Azure 流量管理員會使用各種可用的路由方法來順應基礎結構的不同需求，而有助於為您的組織建立分散各地的高可用性和高效能 AD FS 基礎結構。

高可用性的跨地區 AD FS基礎結構能夠︰

* **消除單一失敗點︰** 利用 Azure 流量管理員的容錯移轉功能，即使全球其中一個資料中心無法使用，您仍可達到高可用性的 AD FS 基礎結構
* **的效能︰** 您可以使用本文中建議的部署，以提供高效能的 AD FS 基礎結構來協助使用者更快驗證。

## <a name="design-principles"></a>設計原則
![整體設計](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

基本設計原則會與＜Azure 中的 AD FS 部署＞一文中所列的設計原則相同。 上圖顯示的是將基本部署擴充至另一個地理區域的簡易延伸模組。 以下是將您的部署擴充到新地理區域時所要考量的一些重點

* **虛擬網路︰** 您應該在您要部署其他 AD FS 基礎結構的地理區域中建立新的虛擬網路。 在上圖中，您會看到 Geo1 VNET 和 Geo2 VNET 顯示為每個地理區中的兩個虛擬網路。
* **新地區 VNET 中的網域控制站和 AD FS 伺服器︰** 建議在新的地理區域中部署網域控制站，在新區域中的 AD FS 伺服器就不需要連絡另一個遙遠網路中的網域控制站來完成驗證，因而改善效能。
* **儲存體帳戶︰** 儲存體帳戶會與某個區域相關聯。 因為您即將在新的地理區域中部署機器，所以您必須建立要在此區域中使用的新儲存體帳戶。
* **網路安全性群組︰** 如同儲存體帳戶，在區域中建立的網路安全性群組不能用於另一個地理區域。 因此，您必須為新地理區域中的 INT 和 DMZ 子網路，建立類似於第一個地理區域中的新網路安全性群組。
* **公用 IP 位址的 DNS 標籤︰** Azure 流量管理員只能透過 DNS 標籤參考端點。 因此，您必須為外部負載平衡器的公用 IP 位址建立 DNS 標籤。
* **Azure 流量管理員** ：Microsoft Azure 流量管理員可讓您控制使用者流量，將流量分散到您在世界各地的不同資料中心執行的服務端點。 Azure 流量管理員是在 DNS 層級上運作。 它會使用 DNS 回應，將使用者流量導向全域散佈的端點。 接著用戶端會直接連接到這些端點。 您可以使用不同的效能、加權和優先順序路由選項，輕鬆地選擇最適合貴組織需求的路由選項。
* **兩個區域之間的 VNet 對 VNet 連線︰** 您不需要具備虛擬網路本身之間的連線。 因為每個虛擬網路都可存取網域控制站，而且本身都有 AD FS 和 WAP 伺服器，所以不同區域中的虛擬網路之間不需要任何連線，即可運作。

## <a name="steps-to-integrate-azure-traffic-manager"></a>整合 Azure 流量管理員的步驟
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>在新的地理區域中部署 AD FS
請依照 [Azure 中的 AD FS 部署](how-to-connect-fed-azure-adfs.md) 中的步驟和指導方針，在新的地理區域中部署相同的拓樸。

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>網際網路對向 (公用) 負載平衡器中公用 IP 位址的 DNS 標籤
如先前所述，Azure 流量管理員只能將 DNS 標籤稱為端點，因此請務必為外部負載平衡器的公用 IP 位址建立 DNS 標籤。 以下螢幕擷取畫面顯示如何設定公用 IP 位址的 DNS 標籤。

![DNS 標籤](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>部署 Azure 流量管理員
依照以下步驟建立流量管理員設定檔。 如需詳細資訊，您也可以參考 [管理 Azure 流量管理員設定檔](/azure/traffic-manager/traffic-manager-manage-profiles)。

1. **建立流量管理員設定檔** ：為您的流量管理員設定檔提供一個唯一的名稱。 設定檔的這個名稱是 DNS 名稱的一部分，並可做為流量管理員網域名稱標籤的前置詞。 系統會將此名稱 / 前置詞新增至 .trafficmanager.net，以建立流量管理員的 DNS 標籤。 以下螢幕擷取畫面顯示設定為 mysts 的流量管理員 DNS 前置詞，而產生的 DNS 標籤會是 mysts.trafficmanager.net。

    ![流量管理員設定檔建立](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **流量路由方法：** 流量管理員中有三個可用的路由選項：

   * 優先順序
   * 效能
   * 加權

     **效能** 是達到高回應性 AD FS 基礎結構的建議選項。 不過，您可以選擇任何最適合您的部署需求的路由方式。 AD FS 功能不會受到所選取的路由選項影響。 如需詳細資訊，請參閱 [流量管理員流量路由方法](/azure/traffic-manager/traffic-manager-routing-methods) 。 在上面的範例螢幕擷取畫面中，您可以看到已選取 **效能** 方法。
3. **設定端點︰** 在流量管理員頁面中，按一下端點並選取 [新增]。 這會開啟類似以下螢幕擷取畫面的 [新增端點] 頁面

   ![設定端點](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)

   對於不同的輸入，請遵循下列指導方針︰

   **類型︰** 選取 Azure 端點，因為我們即將指向 Azure 公用 IP 位址。

   **名稱︰** 建立您想要與端點建立關聯的名稱。 這不是 DNS 名稱，所以與 DNS 記錄沒有關聯。

   **目標資源類型︰** 選取公用 IP 位址做為這個屬性的值。

   **目標資源︰** 這可讓您選擇不同於您訂用帳戶之下可用的 DNS 標籤。 選擇對應到您要設定之端點的 DNS 標籤。

   針對您希望 Azure 流量管理員路由傳送流量的每個地理區域，新增端點。
   如需有關如何在流量管理員中新增 / 設定端點的詳細資訊和詳細步驟，請參閱 [新增、停用、啟用或刪除端點](/azure/traffic-manager/traffic-manager-manage-endpoints)
4. **設定探查︰** 在流量管理員頁面中，按一下 [組態]。 在 [組態] 頁面中，您便需變更監視設定，以在 HTTP 連接埠 80 和相對路徑 /adfs/probe 探查

    ![設定探查](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png)

   > [!NOTE]
   > **確保在設定完成時，端點的狀態會處於線上狀態**。 如果所有端點都處於「降級」狀態，Azure 流量管理員將會嘗試路由傳送流量（假設診斷錯誤），而且所有端點都可以連線。
   >
   >
5. **修改 Azure 流量管理員的 DNS 記錄︰** 您的同盟服務應該是 Azure 流量管理員 DNS 名稱的 CNAME。 在公用 DNS 記錄中建立 CNAME，以便正嘗試觸達同盟服務的人員實際觸達 Azure 流量管理員。

    例如，若要將同盟服務 fs.fabidentity.com 指向流量管理員，您需要將 DNS 資源記錄更新如下：

    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>測試路由和 AD FS 登入
### <a name="routing-test"></a>路由測試
路由的基本測試就是嘗試從每個地理區域中的機器 ping 同盟服務 DNS 名稱。 視所選的路由方法而定，其實際 ping 的端點將會反映在 ping 顯示中。 例如，如果您選取了效能路由，則會觸達最接近用戶端區域的端點。 以下是兩個來自兩個不同區域用戶端電腦的 ping 快照，一個位於 EastAsia 區域，一個位於美國西部。

![路由測試](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>AD FS 登入測試
若要測試 AD FS，最簡單的方法是使用 IdpInitiatedSignon.aspx 網頁。 為了能夠執行此作業，必須在 AD FS 屬性上啟用 IdpInitiatedSignOn。 請遵循下列步驟來確認 AD FS 設定

1. 使用 PowerShell 在 AD FS 伺服器上執行以下 Cmdlet，將它設定為啟用。
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
2. 從任何外部電腦存取 https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. 您應該會看到如下圖的 AD FS 網頁︰

    ![ADFS 測試 - 驗證挑戰](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)

    而成功登入時，它會提供您如下所示的成功訊息︰

    ![ADFS 測試 - 驗證](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>相關連結
* [Azure 中的基本 AD FS 部署](how-to-connect-fed-azure-adfs.md)
* [Microsoft Azure 流量管理員](/azure/traffic-manager/)
* [流量管理員流量路由方法](/azure/traffic-manager/traffic-manager-routing-methods)

## <a name="next-steps"></a>後續步驟
* [管理 Azure 流量管理員設定檔](/azure/traffic-manager/traffic-manager-manage-profiles)
* [新增、停用、啟用或刪除端點](/azure/traffic-manager/traffic-manager-manage-endpoints)
