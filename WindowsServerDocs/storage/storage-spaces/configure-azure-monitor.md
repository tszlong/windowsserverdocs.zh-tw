---
title: 了解與設定 Azure 監視器
description: 詳細安裝資訊，在 Azure 監視器的功能，以及如何在 Windows Server 2016 和 2019年設定您的儲存空間直接存取叢集的電子郵件和 sms 警示。
keywords: 儲存空間直接存取 azure 監視器、 通知、 電子郵件、 sms
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: 6b229696e796f176fe89ab250ab48f1d9f0d5666
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280198"
---
---
# <a name="use-azure-monitor-to-send-emails-for-health-service-faults"></a>使用 Azure 監視器的健全狀況服務錯誤傳送電子郵件

>適用於：Windows Server 2019，Windows Server 2016

Azure 監視器將應用程式的效能與可用性最大化，藉由提供完整的解決方案，來收集、 分析及處理遙測資料從您的雲端和內部部署環境。 它可協助您了解如何執行您的應用程式，並主動識別問題會影響它們和所相依的資源。

這是特別有幫助您在內部部署超交集叢集。 使用 Azure 監視器整合，您可以設定電子郵件、 簡訊 (SMS) 和其他警示，來 ping 您時有問題發生，與您的叢集 （或當您想要收集的資料為基礎的一些其他活動的旗標）。 下面我們將簡短說明 Azure 監視器的運作方式、 如何安裝 Azure 監視器，以及如何將它設定為傳送通知給您。


## <a name="understanding-azure-monitor"></a>了解 Azure 監視器

Azure 監視器所收集的所有資料都放入兩種基本類型之一： 計量和記錄。

1. [計量](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics)是描述某方面的特定時間點的系統時間的數值。 也就是輕量型且能夠支援近乎即時的情境。 您會看到他們在 Azure 入口網站中的 [概觀] 頁面中的 Azure 監視器以滑鼠右鍵所收集的資料。

![計量會內嵌在計量瀏覽器中的映像](media/configure-azure-monitor/metrics.png)

2. [記錄檔](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#logs)包含不同類型的資料組織成不同的每個類型的屬性集的記錄。 例如事件和追蹤遙測會儲存為記錄檔除了效能資料，讓它全都能濃縮進行分析。 Azure 監視器所收集的記錄資料可以在分析後[查詢](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview)快速擷取、 彙總，並分析收集的資料。 您可以建立和測試使用的查詢[Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/portals)在 Azure 入口網站，然後將直接使用這些工具分析資料，或儲存查詢搭配[視覺效果](https://docs.microsoft.com/azure/azure-monitor/visualizations)或[警示規則](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)。

![擷取 log analytics 中的記錄檔的影像](media/configure-azure-monitor/logs.png)

我們有更多詳細資料下, 面有關如何設定這些警示。

## <a name="configuring-health-service"></a>設定健全狀況服務

您只需要第一件事是設定您的叢集。 您可能已經知道，[健全狀況服務](../../failover-clustering/health-service-overview.md)改善的日常監視和操作的經驗，對於執行儲存空間直接存取的叢集。 

如上面的我們所見，Azure 監視器會從其執行您的叢集中的每個節點收集記錄檔。 因此，我們需要設定健全狀況服務寫入到事件通道，這剛好是：

```
Event Channel: Microsoft-Windows-Health/Operational
Event ID: 8465
```

若要設定健全狀況服務，您執行：

```PowerShell
get-storagesubsystem clus* | Set-StorageHealthSetting -Name "Platform.ETW.MasTypes" -Value "Microsoft.Health.EntityType.Subsystem,Microsoft.Health.EntityType.Server,Microsoft.Health.EntityType.PhysicalDisk,Microsoft.Health.EntityType.StoragePool,Microsoft.Health.EntityType.Volume,Microsoft.Health.EntityType.Cluster"
```

當您執行上述設定的健康狀態設定 cmdlet 時，會造成我們想要開始寫入的事件*Microsoft Windows 健康情況/Operational*事件通道。

## <a name="configuring-log-analytics"></a>設定 Log Analytics

現在您已設定適當的登入您的叢集下, 一個步驟就是適當地設定 log analytics。

若要提供概觀[Azure Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows)可以直接從您的實體或虛擬 Windows 電腦在您的資料中心或其他雲端環境中收集資料到單一的存放庫，以供詳細的分析和相互關聯。

若要了解支援的設定，請檢閱[支援的 Windows 作業系統](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems)並[網路防火牆組態](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements)。

如果您還沒有 Azure 訂用帳戶，建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)開始之前。

### <a name="login-in-to-azure-portal"></a>登入 Azure 入口網站

登入 Azure 入口網站，網址[ https://portal.azure.com ](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

### <a name="create-a-workspace"></a>建立工作區

如需下面所列的步驟的詳細資訊，請參閱 < [Azure 監視器文件](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-windows-computer)。

1. 在 Azure 入口網站中，按一下**所有服務**。 在資源清單中，輸入**Log Analytics**。 當您開始輸入時，清單會篩選根據您的輸入。 選取  **Log Analytics**。<br><br> 

   ![Azure 入口網站](media/configure-azure-monitor/azure-portal-01.png)<br><br>

2. 按一下 **建立**，然後選取 下列項目：

   * 提供新名稱**Log Analytics 工作區**，例如*DefaultLAWorkspace*。 
   * 選取 **訂用帳戶**連結到選取的預設值是否不適當，從下拉式清單中選取。
   * 針對**資源群組**，選取現有的資源群組，其中包含一或多個 Azure 虛擬機器。 <br><br>

      ![建立 Log Analytics 資源刀鋒視窗](media/configure-azure-monitor/create-loganalytics-workspace-02.png) <br><br>  

3. 在提供必要的資訊之後**Log Analytics 工作區**窗格中，按一下**確定**。  

雖然會驗證此資訊並建立工作區，您可以底下追蹤其進度**通知**從功能表。 

### <a name="obtain-workspace-id-and-key"></a>取得工作區識別碼和金鑰
然後再安裝 Microsoft Monitoring Agent 的 Windows，您需要的工作區識別碼和金鑰為您的 Log Analytics 工作區。  這項資訊是精靈所需的安裝程式才能正確設定代理程式，並確保與 Log Analytics 可以順利進行通訊。  

1. 在 Azure 入口網站中，按一下**所有服務**位於左上角。 在資源清單中，輸入**Log Analytics**。 當您開始輸入時，清單會篩選根據您的輸入。 選取  **Log Analytics**。
2. 在 Log Analytics 工作區清單中，選取*DefaultLAWorkspace*稍早建立。
3. 選取 **進階設定**。<br><br> ![Log Analytics 進階設定](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br>  
4. 選取 **連接的來源**，然後選取**Windows 伺服器**。   
5. 值，右邊**工作區識別碼**並**主索引鍵**。 兩個都可暫時儲存-複製並貼到您喜好的編輯器，目前兩者。   

## <a name="installing-the-agent-on-windows"></a>在 Windows 上安裝代理程式
下列步驟會安裝並設定 Microsoft Monitoring Agent。 **請務必在叢集中的每部伺服器上安裝此代理程式，並指出您想要在 Windows 啟動時執行的代理程式。**

1. 在  **Windows 伺服器**頁面上，選取適當**下載 Windows 代理程式**根據 Windows 作業系統的處理器架構所下載的版本。
2. 執行您的電腦上安裝代理程式的安裝程式。
2. 在 [歡迎]  頁面上，按 [下一步]  。
3. 在 **授權條款**頁面上，閱讀授權，然後按一下**我同意**。
4. 在 [**目的地資料夾**頁面上，變更或保留預設的安裝資料夾，然後按一下**下一步]** 。
5. 在 [**代理程式安裝選項**頁面上，選擇連接到 Azure Log Analytics 的代理程式，然後按一下**下一步]** 。   
6. 在  **Azure Log Analytics**頁面上，執行下列動作：
   1. 貼上**工作區識別碼**並**工作區金鑰 （主索引鍵）** 您之前複製。    
    a. 如果電腦需要透過 proxy 伺服器與 Log Analytics 服務進行通訊，請按一下**進階**和提供的 URL 和連接埠號碼的 proxy 伺服器。  如果您的 proxy 伺服器需要驗證，請輸入使用者名稱和密碼向 proxy 伺服器，然後按一下**下一步**。  
7. 按一下 [**下一步]** 提供必要的組態設定完成之後。<br><br> ![貼上 工作區識別碼和主索引鍵](media/configure-azure-monitor/log-analytics-mma-setup-laworkspace.png)<br><br>
8. 在 **安裝準備就緒**頁面上，檢閱您的選擇，然後按一下**安裝**。
9. 在 **組態已順利完成**頁面上，按一下**完成**。

完成時， **Microsoft Monitoring Agent**會出現在**控制台**。 您可以檢閱您的設定，並確認代理程式已連線到 Log Analytics。 當連線時，在**Azure Log Analytics**索引標籤上，代理程式會顯示訊息：**Microsoft Monitoring Agent 已成功連線到 Microsoft Log Analytics 服務。** 

![MMA 對 Log Analytics 的連線狀態](media/configure-azure-monitor/log-analytics-mma-laworkspace-status.png)

若要了解支援的設定，請檢閱[支援的 Windows 作業系統](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems)並[網路防火牆組態](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements)。

## <a name="collecting-event-and-performance-data"></a>收集事件和效能資料

Log Analytics 可以從您指定較長期分析和報告的效能計數器與 Windows 事件記錄檔收集事件，並在偵測到特定條件時採取動作。  請遵循下列步驟來開始設定收集的 Windows 事件記錄檔，以及數個常用的效能計數器事件。  

1. 在 Azure 入口網站中，按一下**更多服務**左上角。 在資源清單中，輸入**Log Analytics**。 當您開始輸入時，清單會篩選根據您的輸入。 選取  **Log Analytics**。
2. 選取 **進階設定**。<br><br> ![Log Analytics 進階設定](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br> 
3. 選取 **資料**，然後選取**Windows 事件記錄檔**。  
4. 在這裡，輸入下列名稱，然後按一下加號新增健全狀況服務事件通道 **+** 。  
   ```
   Event Channel: Microsoft-Windows-Health/Operational
   ```
5. 在資料表中，請檢查的嚴重性**錯誤**並**警告**。   
6. 按一下 **儲存**頂端的 儲存設定 頁面。
7. 選取  **Windows 效能計數器**啟用收集 Windows 電腦上的效能計數器。 
8. 當您第一次設定新的 Log Analytics 工作區的 Windows 效能計數器時，您可以快速建立數個常用的計數器的選項。 它們會列出每個旁邊的核取方塊。<br> ![選取的預設 Windows 效能計數器](media/configure-azure-monitor/windows-perfcounters-default.png)<br> 按一下 **新增選取的效能計數器**。  隨即會新增且預設使用 10 秒收集取樣間隔時間。  
9. 按一下 **儲存**頂端的 儲存設定 頁面。

## <a name="creating-alerts-based-on-log-data"></a>建立記錄檔資料為基礎的警示

如果您已將它這到目前為止，您的叢集應該會傳送您的記錄檔和效能計數器到 Log Analytics。 下一個步驟是建立警示規則，自動定期執行記錄搜尋。 如果記錄搜尋結果符合特定準則，然後引發警示，傳送電子郵件或文字通知給您。 讓我們來探索這下面。

### <a name="create-a-query"></a>建立查詢

首先開啟記錄搜尋入口網站。   

1. 在 Azure 入口網站中，按一下**所有服務**。 在資源清單中，輸入**監視器**。 當您開始輸入時，清單會篩選根據您的輸入。 選取 **監視器**。
2. 在 監視 瀏覽功能表中，選取  **Log Analytics** ，然後選取 工作區。

擷取要使用的某些資料最快的方式是簡單的查詢來傳回資料表中的所有記錄。 在搜尋方塊中輸入下列查詢，然後按一下 [搜尋] 按鈕。  

```
Event
```

在預設清單檢視中，會傳回資料，您可以看到傳回的多少的記錄總數。

![簡單的查詢](media/configure-azure-monitor/log-analytics-portal-eventlist-01.png)

在畫面左側是篩選窗格，可讓您將篩選加入查詢而不需直接修改它。  數個記錄的屬性會顯示該記錄的類型，您可以選取一或多個屬性值，以縮小搜尋結果。

選取此核取方塊旁**錯誤**下方**EVENTLEVELNAME**或輸入下列命令以將結果限制為錯誤事件。

```
Event | where (EventLevelName == "Error")
```

![篩選](media/configure-azure-monitor/log-analytics-portal-eventlist-02.png)

對您想知道事件的適當查詢之後，請將它們儲存在下一個步驟。

### <a name="create-alerts"></a>建立警示
現在，讓我們逐步解說建立警示的範例。

1. 在 Azure 入口網站中，按一下**所有服務**。 在資源清單中，輸入**Log Analytics**。 當您開始輸入時，清單會篩選根據您的輸入。 選取  **Log Analytics**。
2. 在左側窗格中，選取**警示**，然後按一下**新的警示規則**頂端的頁面，即可建立新的警示。<br><br> ![建立新的警示規則](media/configure-azure-monitor/alert-rule-02.png)<br>
3. 第一個步驟中，如底下**建立警示** 區段中，您要的資源中，選取您的 Log Analytics 工作區，因為這是記錄檔為基礎的警示訊號。  篩選結果的特定**訂用帳戶**從下拉式清單，如果您有多個，其中包含稍早建立的 Log Analytics 工作區。  篩選條件**資源類型**藉由選取**Log Analytics**從下拉式清單。  最後，選取**Resource** **DefaultLAWorkspace** ，然後按一下 **完成**。<br><br> ![建立警示的步驟 1 的工作](media/configure-azure-monitor/alert-rule-03.png)<br>
4. 下一節**警示準則**，按一下**將準則新增**選取您已儲存的查詢，然後指定 警示規則所遵循的邏輯。
5. 使用下列資訊來設定警示：  
   a. 從**根據**下拉式清單中，選取**計量測量**。  計量測量會建立每個物件的警示，在查詢中的值，超過我們指定的臨界值。  
   b. 針對**條件**，選取**大於**並指定 thershold。  
   c. 然後，定義何時要觸發警示。 例如，您可以選擇**連續違規**然後從下拉式清單中選取**大於**值為 3。  
   d. 下一節所根據的評估，修改**期間**值**30**分鐘並**頻率**為 5。 此規則會每隔五分鐘執行一次，並傳回已從目前的時間在過去 30 分鐘內建立的記錄。  更多的視窗中設定的時間週期負責資料延遲的可能性，並確保查詢會傳回資料，以避免 (false negative) 永遠不會引發警示。  
6. 按一下 **完成**完成警示規則。<br><br> ![設定警示訊號](media/configure-azure-monitor/alert-signal-logic-02.png)<br> 
7. 現在將移至第二個步驟中，提供您的警示中的名稱**警示規則名稱**欄位，例如**上的所有錯誤事件警示**。  指定**描述**具體警示，然後選取**重大 (Sev 0)** for**嚴重性**從提供的選項值。
8. 若要立即啟用警示規則建立時，接受預設值**在建立時啟用規則**。
9. 第三個也是最後一個步驟中，針對您指定**動作群組**，以確保每次觸發警示，並可用於您定義每個規則之後，會採取相同的動作。 使用下列資訊來設定新的動作群組：  
   a. 選取 **新動作群組**並**新增動作群組**窗格隨即出現。  
   b. 針對**動作群組名稱**，指定名稱，例如**IT 作業-通知**和**簡短名稱**例如**itops n**。  
   c. 驗證的預設值**訂用帳戶**並**資源群組**正確無誤。 否則，請選取正確的密碼，從下拉式清單。   
   d. 在 [動作] 區段中，指定動作的名稱這類**傳送電子郵件**下方，並在**動作類型**選取**電子郵件/簡訊/推送/語音**從下拉式清單。 **電子郵件/簡訊/推送/語音**屬性 窗格會開啟以提供其他資訊的權限。  
   e. 在 [**電子郵件/簡訊/推送/語音**] 窗格中，選取並設定您的喜好設定。 例如，啟用**電子郵件**，並提供有效的電子郵件傳遞到郵件的 SMTP 地址。  
   f. 按一下 [確定]  以儲存您的變更。<br><br> 

    ![建立新動作群組](media/configure-azure-monitor/action-group-properties-01.png)

10. 按一下 **確定**完成的動作群組。 
11. 按一下 **建立警示規則**完成警示規則。 它會立即開始執行。<br><br> ![完成 建立新的警示規則](media/configure-azure-monitor/alert-rule-01.png)<br> 

### <a name="test-alerts"></a>測試警示

如需參考，這是範例警示的樣子：

![警示的電子郵件範例](media/configure-azure-monitor/warning.png)

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)
- 如需詳細資訊，請閱讀[Azure 監視器文件](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata)。
