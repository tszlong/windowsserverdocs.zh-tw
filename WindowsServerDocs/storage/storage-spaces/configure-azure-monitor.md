---
title: 瞭解和設定 Azure 監視器
description: Azure 監視器的詳細設定資訊，以及如何在 Windows Server 2016 和2019中設定儲存空間直接存取叢集的電子郵件和 sms 警示。
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/10/2020
ms.openlocfilehash: fa4d8793c07cabd39ee6cc0d54b5cddc84eac202
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859041"
---
# <a name="use-azure-monitor-to-send-emails-for-health-service-faults"></a>使用 Azure 監視器來傳送健全狀況服務錯誤的電子郵件

>適用于： Windows Server 2019、Windows Server 2016

Azure 監視器可藉由提供全方位的解決方案，以便收集、分析及處理來自雲端和內部部署環境的遙測資料，進而將應用程式的可用性和效能最大化。 它可協助您了解應用程式的執行情況，並主動識別影響它們的問題以及它們所依賴的資源。

這對您的內部部署超融合式叢集特別有用。 在 Azure 監視器整合後，您將能夠設定電子郵件、文字（SMS）和其他警示，以便在叢集發生問題時 ping 您（或者，當您想要根據所收集的資料來標示一些其他活動時）。 以下將簡短說明 Azure 監視器的運作方式、如何安裝 Azure 監視器，以及如何設定它來傳送通知給您。

如果您使用的是 System Center，請查看同時監視 Windows Server 2019 和 Windows Server 2016 儲存空間直接存取叢集的[儲存空間直接存取管理元件](https://www.microsoft.com/download/details.aspx?id=100782)。

此管理組件包含：

* 實體磁片健全狀況和效能監視
* 存放裝置節點健全狀況和效能監視
* 存放集區健全狀況和效能監視
* 磁片區復原類型與重復資料刪除狀態

## <a name="understanding-azure-monitor"></a>瞭解 Azure 監視器

Azure 監視器所收集的所有資料都符合下列兩種基本類型的其中一種：計量和記錄。

1. [計量](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics)是數值，可描述系統在特定時間點的某些層面。 它們屬於輕量型，而且能夠支援近乎即時的案例。 您會在 Azure 入口網站的 [總覽] 頁面中，看到 Azure 監視器所收集的資料。

![計量瀏覽器中的計量內嵌影像](media/configure-azure-monitor/metrics.png)

2. [記錄包含不同](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#logs)種類的資料，組織成每種類型各有不同的屬性集。 除了效能資料，還會將事件和追蹤之類的遙測資料儲存為記錄，讓它能夠全部組合在一起進行分析。 您可以使用[查詢](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview)來分析 Azure 監視器所收集的記錄資料，以快速取得、合併和分析所收集的資料。 您可以使用 Azure 入口網站中的[Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/portals)來建立及測試查詢，然後使用這些工具直接分析資料，或儲存查詢以搭配[視覺效果](https://docs.microsoft.com/azure/azure-monitor/visualizations)或[警示規則](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)使用。

![log analytics 中內嵌的記錄影像](media/configure-azure-monitor/logs.png)

我們將在下面詳細說明如何設定這些警示。

## <a name="onboarding-your-cluster-using-windows-admin-center"></a>使用 Windows 管理中心將叢集上線

使用 Windows 系統管理中心，您可以將叢集上架至 Azure 監視器。

![將叢集上架到 Azure 監視器的 Gif](media/configure-azure-monitor/onboarding.gif)

在此上線流程期間，下列步驟會在幕後發生。 我們會詳細說明如何在您想要手動設定叢集時詳細設定它們。 

### <a name="configuring-health-service"></a>設定健全狀況服務

您需要做的第一件事，就是設定您的叢集。 您可能已經知道，[健全狀況服務](../../failover-clustering/health-service-overview.md)可以改善執行儲存空間直接存取之叢集的日常監視和操作體驗。 

如先前所見，Azure 監視器會從叢集中執行的每個節點收集記錄。 因此，我們必須將健全狀況服務設定為寫入事件通道，這剛好是：

```
Event Channel: Microsoft-Windows-Health/Operational
Event ID: 8465
```

若要設定健全狀況服務，請執行：

```PowerShell
get-storagesubsystem clus* | Set-StorageHealthSetting -Name "Platform.ETW.MasTypes" -Value "Microsoft.Health.EntityType.Subsystem,Microsoft.Health.EntityType.Server,Microsoft.Health.EntityType.PhysicalDisk,Microsoft.Health.EntityType.StoragePool,Microsoft.Health.EntityType.Volume,Microsoft.Health.EntityType.Cluster"
```

當您執行上述 Cmdlet 來設定健康情況設定時，會導致我們想要開始寫入*Microsoft Windows-健全狀況/操作*事件通道的事件。

### <a name="configuring-log-analytics"></a>設定 Log Analytics

既然您已在叢集上設定適當的記錄，下一步就是正確地設定 log analytics。

為了提供總覽， [Azure Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows)可直接從您資料中心或其他雲端環境中的實體或虛擬 Windows 電腦，將資料收集到單一存放庫，以進行詳細的分析和相互關聯。

若要瞭解支援的設定，請參閱[支援的 Windows 作業系統](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems)和[網路防火牆](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements)設定。

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

#### <a name="login-in-to-azure-portal"></a>登入 Azure 入口網站

登入位於[https://portal.azure.com](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)的 Azure 入口網站。

#### <a name="create-a-workspace"></a>建立工作區

如需下面所列步驟的詳細資訊，請參閱[Azure 監視器檔](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-windows-computer)。

1. 在 Azure 入口網站中，按一下 **所有服務**。 在資源清單中，輸入**Log Analytics**。 當您開始輸入時，清單會根據您的輸入來篩選。 選取 [ **Log Analytics**]。<br><br> 

   ![Azure 入口網站](media/configure-azure-monitor/azure-portal-01.png)<br><br>

2. 按一下 [**建立**]，然後選取下列專案的選項：

   * 提供新**Log Analytics 工作區**的名稱，例如*DefaultLAWorkspace*。 
   * 如果選取的預設值不合適，請從下拉式清單中選取要連結的**訂**用帳戶。
   * 針對 [**資源群組**]，選取包含一或多個 Azure 虛擬機器的現有資源群組。 <br><br>

      ![[建立 Log Analytics] 資源刀鋒視窗](media/configure-azure-monitor/create-loganalytics-workspace-02.png) <br><br>  

3. 在 [ **Log Analytics 工作區**] 窗格上提供必要資訊之後，按一下 **[確定]** 。  

在驗證資訊並建立工作區時，您可以在功能表的 [**通知**] 底下追蹤其進度。 

#### <a name="obtain-workspace-id-and-key"></a>取得工作區識別碼和金鑰
安裝適用於 Windows 的 Microsoft Monitoring Agent 之前，您必須有 Log Analytics 工作區的工作區識別碼和金鑰。  安裝精靈必須要有這項資訊，才能正確設定代理程式，並確保它能與 Log Analytics 順利進行通訊。  

1. 在 Azure 入口網站中，按一下位於左上角的 **所有服務**。 在資源清單中，輸入**Log Analytics**。 當您開始輸入時，清單會根據您的輸入來篩選。 選取 [ **Log Analytics**]。
2. 在您的 Log Analytics 工作區清單中，選取稍早建立的*DefaultLAWorkspace* 。
3. 選取 [ **Advanced settings**]。<br><br> ![Log Analytics 的前進設定](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br>  
4. 選取 [**連線的來源**]，然後選取 [ **Windows 伺服器**]。   
5. [**工作區識別碼**] 和 [**主要金鑰**] 右邊的值。 將暫時複製並貼到您最愛的編輯器中，以節省時間。   

### <a name="installing-the-agent-on-windows"></a>在 Windows 上安裝代理程式
下列步驟會安裝和設定 Microsoft Monitoring Agent。 **請務必在叢集中的每部伺服器上安裝此代理程式，並指出您要讓代理程式在 Windows 啟動時執行。**

1. 在 [ **Windows 伺服器**] 頁面上，根據 Windows 作業系統的處理器架構，選取適當的 [**下載 Windows 代理程式**] 版本來下載。
2. 執行安裝程式以在您的電腦上安裝代理程式。
2. 在 [歡迎] 頁面上，按 [下一步]。
3. 閱讀 [**授權條款**] 頁面上的授權，然後按一下 [**我同意**]。
4. 在 [**目的地資料夾**] 頁面上，變更或保留預設的安裝資料夾，然後按 **[下一步]** 。
5. 在 [**代理程式安裝選項**] 頁面上，選擇將代理程式連線至 Azure Log Analytics，然後按 **[下一步]** 。   
6. 在 [ **Azure Log Analytics** ] 頁面上，執行下列動作：
   1. 貼上您稍早複製的**工作區識別碼**和**工作區金鑰（主要金鑰）** 。    
    a. 如果電腦需要透過 proxy 伺服器與 Log Analytics 服務通訊，請按一下 [ **Advanced** ]，並提供 proxy 伺服器的 URL 和埠號碼。  如果您的 proxy 伺服器需要驗證，請輸入要向 proxy 伺服器驗證的使用者名稱和密碼，然後按 **[下一步]** 。  
7. 完成提供必要的設定之後，請按 **[下一步]** 。<br><br> ![貼上工作區識別碼和主要金鑰](media/configure-azure-monitor/log-analytics-mma-setup-laworkspace.png)<br><br>
8. 在 [**安裝準備就緒**] 頁面上，檢查您的選擇，然後按一下 [**安裝**]。
9. 在 [設定已**成功完成**] 頁面上，按一下 **[完成]** 。

完成時， **Microsoft Monitoring Agent**會出現在 [**控制台**] 中。 您可以檢閱您的組態，並確認代理程式已連線到 Log Analytics。 連線時，在 [ **Azure Log Analytics** ] 索引標籤上，代理程式會顯示訊息，指出**Microsoft Monitoring Agent 已成功連線到 Microsoft Log Analytics 服務。** 

![MMA 對 Log Analytics 的連線狀態](media/configure-azure-monitor/log-analytics-mma-laworkspace-status.png)

若要瞭解支援的設定，請參閱[支援的 Windows 作業系統](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems)和[網路防火牆](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements)設定。

## <a name="setting-up-alerts-using-windows-admin-center"></a>使用 Windows 管理中心設定警示

在 Windows 系統管理中心，您可以設定預設警示，以套用至 Log Analytics 工作區中的所有伺服器。 

![設定警示的 Gif](media/configure-azure-monitor/setup1.gif)

這些是您可以加入宣告的警示和其預設條件：

| 警示名稱                | 預設條件                                  |
|---------------------------|----------------------------------------------------|
| CPU 使用率           | 10分鐘超過85%                            |
| 磁片容量使用率 | 10分鐘超過85%                            |
| 記憶體使用率        | 10分鐘內的可用記憶體小於 100 MB   |
| 活動訊號                 | 5分鐘少於2個節拍                   |
| 系統嚴重錯誤     | 叢集系統事件記錄檔中的任何重大警示 |
| 健全狀況服務警示      | 叢集上的任何健全狀況服務錯誤            |

在 Windows 管理中心設定警示之後，您就可以在 Azure 中的 log analytics 工作區中看到警示。

![設定警示的 Gif](media/configure-azure-monitor/setup2.gif)

在此上線流程期間，下列步驟會在幕後發生。 我們會詳細說明如何在您想要手動設定叢集時詳細設定它們。 

### <a name="collecting-event-and-performance-data"></a>收集事件和效能資料

Log Analytics 可以從 Windows 事件記錄檔收集事件，以及收集您針對長期分析和報告指定的效能計數器，並在偵測到特定條件時採取動作。  請依照下列步驟來設定從 Windows 事件記錄檔收集事件，以及收集數個常用的效能計數器，來開始進行作業。  

1. 在 Azure 入口網站中，按一下左下角的 **更多服務**。 在資源清單中，輸入**Log Analytics**。 當您開始輸入時，清單會根據您的輸入來篩選。 選取 [ **Log Analytics**]。
2. 選取 [ **Advanced settings**]。<br><br> ![Log Analytics 的前進設定](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br> 
3. 選取 [**資料**]，然後選取 [ **Windows 事件記錄**]。  
4. 在這裡，輸入下方的名稱，然後按一下加號 **+** ，以新增健全狀況服務事件通道。  
   ```
   Event Channel: Microsoft-Windows-Health/Operational
   ```
5. 在資料表中，檢查嚴重性**錯誤**和**警告**。   
6. 按一下頁面頂端的 [**儲存**] 以儲存設定。
7. 選取 [ **Windows 效能計數器**] 以啟用收集 windows 電腦上的效能計數器。 
8. 當您第一次為新的 Log Analytics 工作區設定 Windows 效能計數器時，系統會提供選項，讓您快速建立數個常用的計數器。 這些計數器旁邊皆會列出核取方塊。<br> ![選取的預設 Windows 效能計數器](media/configure-azure-monitor/windows-perfcounters-default.png)<br> 按一下 **[新增選取的效能計數器**]。  隨即會新增且收集取樣間隔時間的預設值為 10 秒。  
9. 按一下頁面頂端的 [**儲存**] 以儲存設定。

## <a name="creating-alerts-based-on-log-data"></a>根據記錄資料建立警示

如果您已經這麼做，叢集應該會將記錄檔和效能計數器傳送至 Log Analytics。 下一步是建立會自動定期執行記錄搜尋的警示規則。 如果記錄搜尋的結果符合特定準則，則會引發警示，傳送電子郵件或文字通知給您。 讓我們來探索下面的程式。

### <a name="create-a-query"></a>建立查詢

從開啟記錄搜尋入口網站開始。   

1. 在 Azure 入口網站中，按一下 **所有服務**。 在資源清單中，輸入**Monitor**。 當您開始輸入時，清單會根據您的輸入來篩選。 選取 [**監視**]。
2. 在 [監視器] 導覽功能表上，選取 [ **Log Analytics** ]，然後選取工作區。

若要擷取某些資料來使用，最快的方式是使用會傳回資料表中所有記錄的簡單查詢。 在搜尋方塊中輸入下列查詢，然後按一下 [搜尋] 按鈕。  

```
Event
```

資料會傳回到預設清單檢視中，您可以看到傳回的記錄總數。

![簡單查詢](media/configure-azure-monitor/log-analytics-portal-eventlist-01.png)

畫面左側是篩選窗格，可讓您新增篩選條件到查詢中，而不需要直接修改查詢。  對於該記錄類型會顯示數個記錄屬性，您可以選取一個或多個屬性值，以縮小搜尋結果。

選取 [ **EVENTLEVELNAME** ] 下的 [**錯誤**] 旁的核取方塊，或輸入下列各項，將結果限制為錯誤事件。

```
Event | where (EventLevelName == "Error")
```

![篩選器](media/configure-azure-monitor/log-analytics-portal-eventlist-02.png)

在您針對所關心的事件進行 approriate 查詢之後，請加以儲存以供下一個步驟使用。

### <a name="create-alerts"></a>建立警示
現在，讓我們逐步解說建立警示的範例。

1. 在 Azure 入口網站中，按一下 **所有服務**。 在資源清單中，輸入**Log Analytics**。 當您開始輸入時，清單會根據您的輸入來篩選。 選取 [ **Log Analytics**]。
2. 在左側窗格中，選取 [**警示**]，然後按一下頁面頂端的 [**新增警示規則**] 來建立新的警示。<br><br> ![建立新的警示規則](media/configure-azure-monitor/alert-rule-02.png)<br>
3. 在第一個步驟中，您會在 [**建立警示**] 區段下選取您的 log Analytics 工作區作為資源，因為這是以記錄為基礎的警示信號。  如果您有多個訂用帳戶，其中包含稍早建立的 Log Analytics 工作區，請從下拉式清單中選擇特定的**訂**用帳戶來篩選結果。  從下拉式清單中選取 [ **Log Analytics** ]，以篩選**資源類型**。  最後，選取**資源** **DefaultLAWorkspace** ，然後按一下 [**完成**]。<br><br> ![建立警示步驟1工作](media/configure-azure-monitor/alert-rule-03.png)<br>
4. 在 [**警示準則**] 區段下，按一下 [**新增準則**] 以選取已儲存的查詢，然後指定警示規則所遵循的邏輯。
5. 使用下列資訊設定警示：  
   a. 從 [**根據**] 下拉式清單中，選取 [**度量度量**]。  計量測量會針對查詢中其值超過指定閾值的每個物件建立警示。  
   b. 針對 [**條件**]，選取 [**大於**] 並指定 thershold。  
   c. 然後定義觸發警示的時機。 例如，您可以選取 [**連續違規**]，然後從下拉式清單中選取 [**大於**3 的值]。  
   d. 在 [評估依據] 區段下，將 [**期間**] 值修改為**30**分鐘，**頻率**為5。 此規則會每隔五分鐘執行一次，並傳回在目前時間之前的三十分鐘內建立的記錄。  將時間週期設定為較大的時間範圍，可因應資料延遲的可能性，並確保查詢會傳回資料，以避免發生永遠不會引發警示的漏判。  
6. 按一下 [**完成**] 以完成警示規則。<br><br> ![設定警示信號](media/configure-azure-monitor/alert-signal-logic-02.png)<br> 
7. 現在移至第二個步驟，在 [**警示規則名稱**] 欄位中提供警示的名稱，例如**所有錯誤事件的警示**。  指定警示詳細資訊的**描述**，並從提供的選項中選取 [**重大] （嚴重性0）** 作為 [**嚴重性**] 值。
8. 若要在建立時立即啟用警示規則，請接受 [在**建立時啟用規則**] 的預設值。
9. 在第三個和最後一個步驟中，您指定了**動作群組**，這可確保每次觸發警示時都會採取相同的動作，而且可以用於您定義的每個規則。 使用下列資訊來設定新的動作群組：  
   a. 選取 [**新增動作群組**]，[**加入動作群組**] 窗格隨即出現。  
   b. 針對 [**動作組名**]，指定名稱（例如**IT 作業通知**）和**簡短名稱**（例如**itops-n**）。  
   c. 確認**訂**用帳戶和**資源群組**的預設值是正確的。 如果不正確，從下拉式清單中選取正確的項目。   
   d. 在 [動作] 區段底下，指定動作的名稱，例如 [**傳送電子郵件**]，然後在 [**動作類型**] 下，從下拉式清單中選取 [**電子郵件/SMS/推播/語音**]。 [**電子郵件/SMS/推播/語音**內容] 窗格會在右側開啟，以便提供其他資訊。  
   e. 在 [**電子郵件/SMS/推播/語音**] 窗格中，選取並設定您的喜好設定。 例如，啟用**電子郵件**並提供有效的電子郵件 SMTP 位址，將訊息傳遞到。  
   f. 按一下 **[確定]** 儲存變更。<br><br> 

    ![建立新的動作群組](media/configure-azure-monitor/action-group-properties-01.png)

10. 按一下 **[確定]** 以完成動作群組。 
11. 按一下 [**建立警示規則**] 以完成警示規則。 此警示規則會立即開始執行。<br><br> ![完成建立新的警示規則](media/configure-azure-monitor/alert-rule-01.png)<br> 

### <a name="example-alert"></a>範例警示

如需參考，這是 Azure 中範例警示的外觀。

![Azure 中的警示 Gif](media/configure-azure-monitor/alert.gif)

以下是您將會 Azure 監視器傳送的電子郵件範例：

![警示電子郵件範例](media/configure-azure-monitor/warning.png)

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
- 如需詳細資訊，請閱讀[Azure 監視器檔](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata)。
- 如需如何[連接至其他 Azure 混合式服務](../../manage/windows-admin-center/azure/index.md)的總覽，請參閱此。
