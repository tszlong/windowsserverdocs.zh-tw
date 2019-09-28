---
title: 微調 SQL 並解決 AD FS 的延遲問題
description: 本檔討論如何使用 AD FS 微調 SQL。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 06/20/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 785ecd4de86c06dd12eb57e41efaa1103f2afdc5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357810"
---
# <a name="fine-tuning-sql-and-addressing-latency-issues-with-ad-fs"></a>微調 SQL 並解決 AD FS 的延遲問題
在[AD FS 2016](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294)的更新中，我們引進了下列改良功能來減少跨資料庫的延遲。 即將推出的 AD FS 2019 更新將包含這些改良功能。

## <a name="in-memory-cache-update-in-background-thread"></a>背景執行緒中的記憶體內部快取更新 
在先前的 Alwayson 可用性（AoA）部署中，任何「讀取」作業都存在延遲，因為主要節點可能位於不同的資料中心。 兩個不同資料中心之間的呼叫會產生延遲。  

在 AD FS 的最新更新中，降低延遲的目標是透過新增背景執行緒來重新整理 AD FS 設定快取，以及設定重新整理時間週期的設定。 在要求執行緒中，資料庫查閱所花費的時間會大幅減少，因為資料庫快取更新會移到背景執行緒中。  

`backgroundCacheRefreshEnabled`當設定為 true 時，AD FS 會讓背景執行緒執行快取更新。 藉由設定`cacheRefreshIntervalSecs`，您可以將從快取中提取資料的頻率自訂為時間值。 當設定為 true 時`backgroundCacheRefreshEnabled` ，預設值會設定為300秒。 在設定值持續時間之後，AD FS 開始重新整理它的快取，而當更新正在進行時，將會繼續使用舊的快取資料。  

>[!NOTE]
> 如果 ADFS 收到來自 SQL 的通知，表示資料庫`cacheRefreshIntervalSecs`中發生了變更，則快取的資料會在值之外重新整理。 此通知會觸發重新整理快取。 

### <a name="recommendations-for-setting-the-cache-refresh"></a>設定快取重新整理的建議 
快取重新整理的預設值為**5 分鐘**。 建議您將它設定為**1 小時**，以 AD FS 減少不必要的資料重新整理，因為如果發生任何 SQL 變更，就會重新整理快取資料。  

AD FS 會註冊 SQL 變更的回呼，而在變更時，ADFS 會收到通知。 藉由這個方法，ADFS 會在 SQL 每次發生變更時，立即收到新的變更。 

發生網路問題而導致 AD FS 遺失 SQL 通知時，AD FS 會依照快取重新整理值所指定的間隔重新整理。 如果 AD FS 和 SQL 之間有任何連線問題，建議您將快取重新整理值設定為低於1小時。  

### <a name="configuration-instructions"></a>設定指示 
設定檔支援多個快取專案。 下列列出的全部都可以根據您組織的需求進行設定。 

下列範例會啟用背景快取重新整理，並將快取重新整理期間設定為1800秒或30分鐘。 這必須在每個 ADFS 節點上完成，而且 ADFS 服務必須在之後重新開機。 這些變更不會影響其他節點，而且在所有節點中進行變更之前，會先測試第一個節點。 

  1. 流覽至 AD FS 的設定檔，並在 "IdentityServer" 區段下新增下列專案：  
  
  - `backgroundCacheRefreshEnabled`-指定是否啟用背景快取功能。 "true/false" 值。
  - `cacheRefreshIntervalSecs`-ADFS 會重新整理快取的值（以秒為單位）。 如果 SQL 中有任何變更，AD FS 會重新整理快取。 AD FS 將會收到 SQL 通知，並重新整理快取。  
 
 >[!NOTE]
 > 設定檔中的所有專案都會區分大小寫。  
 &lt;cache cacheRefreshIntervalSecs = "1800" backgroundCacheRefreshEnabled = "true"/&gt; 
 
其他支援的可設定值： 

   - **maxRelyingPartyEntries** -AD FS 將保留在記憶體中的信賴憑證者專案數上限。 OAuth 應用程式許可權快取也會使用此值。 如果應用程式許可權多於 RPs，而且全部都儲存在記憶體中，則此值應為應用程式許可權的數目。 預設值為1000。
   - **maxIdentityProviderEntries** -這是 AD FS 將保留在記憶體中的宣告提供者專案數上限。 預設值為200。 
   - **maxClientEntries** -這是 AD FS 將保留在記憶體中的 OAuth 用戶端專案數上限。 預設值為 500。 
   - **maxClaimDescriptorEntries** -AD FS 將保留在記憶體中的宣告描述項專案數上限。 預設值為 500。 
   - **maxNullEntries** -這會用來作為負數快取。 當 AD FS 在資料庫中尋找某個專案，但找不到它時，AD FS 會加入負快取。 這是該快取的大小上限。 每種類型的物件都有負數快取，但它不是所有物件的單一快取。 預設值為50，0000。 

## <a name="multiple-artifact-db-support-across-datacenters"></a>跨資料中心的多個成品 DB 支援 
針對多個資料中心的先前設定，AD FS 只支援單一成品資料庫，因此會在抓取呼叫期間導致跨中心的資料中心延遲。  

為了減少跨資料中心的延遲，AD FS 系統管理員現在可以部署多個成品 DB 實例，然後將 AD FS 節點的設定檔修改為指向不同的成品 DB 實例。 可以在設定檔中提供成品資料庫連接字串，以允許每個節點的成品 DB。 如果連接字串不存在於設定檔中，節點將會切換回先前的設計，以使用在設定資料庫中的成品資料庫。  
此設定也支援混合式環境。  

### <a name="requirements"></a>需求： 
在設定多個成品資料庫支援之前，請先在所有節點上執行更新，並更新二進位檔，因為多節點呼叫會透過此功能進行。 
  1. 產生部署腳本以建立成品 DB：若要部署多個成品 DB 實例，系統管理員必須產生成品 DB 的 SQL 部署腳本。 在此更新過程中，現有`Export-AdfsDeploymentSQLScript`的 Cmdlet 已更新為可選擇性地採用參數，以指定要產生 SQL 部署腳本的 AD FS 資料庫。 
 
 例如，若要只針對成品 DB 產生部署腳本，請指定`-DatabaseType`參數，並傳入 "成品" 值。 選擇性`-DatabaseType`參數會指定 AD FS 資料庫類型，而且可以設定為：All （預設值）、成品或設定。 如果未`-DatabaseType`指定任何參數，腳本會同時設定成品和設定腳本。  

   ```PowerShell
   PS C:\> Export-AdfsDeploymentSQLScript -DestinationFolder <script folder where scripts will be created> -ServiceAccountName <domain\serviceaccount> -DatabaseType "Artifact" 
   ```
產生的腳本應該在 SQL 電腦上執行，以建立所需的資料庫，並將 SQL SA 許可權授與 AD FS 的服務帳戶。

 2. 使用部署腳本建立成品 DB。 將新產生的 CreateDB 和 SetPermissions sql 部署腳本複製到 SQL server 電腦上，然後執行它們來建立本機成品 DB。 
 
 3. 修改設定檔以新增成品 DB 連接。 
 流覽至 [AD FS] 節點的設定檔，然後在 [IdentityServer] 區段下，將進入點新增至新設定的 ArtifactDB。 

 >[!NOTE] 
 > artifactStore 和 connectionString 是區分大小寫的值。 請確定已正確設定它們。 &lt;artifactStore connectionString = "Data Source = .\SQLInstance; 整合式安全性 = True; 初始目錄 = AdfsArtifactStore"/&gt; 
>
>使用與您的 sql 連接相符的資料來源值。



 4. 重新開機 AD FS 服務，變更才會生效。 
 
 >[!NOTE] 
 > 不建議在成品資料庫之間使用 SQL 複寫或同步處理。 建議您為每個資料中心設定一個成品資料庫。 
 
### <a name="cross-datacenter-failover-and-database-recovery"></a>跨資料中心容錯移轉和資料庫復原  
建議您在與主要成品資料庫相同的資料中心上建立容錯移轉成品資料庫。 如果發生容錯移轉，延遲就不會增加。 不建議跨資料中心的容錯移轉成品資料庫。 下列詳細說明如何使用多個成品資料庫呼叫 OAuth、SAML、ESL 和權杖重新執行偵測功能。 
 - **OAuth 和 SAML** 

   對於 OAuth 和 SAML 成品要求，節點會在設定檔中的成品 DB 中建立成品。 如果設定檔案不包含成品資料庫連接，則會使用一般成品 DB。 下一次提取成品的要求移至另一個節點時，另一個節點會將 rest API 設為第1個節點，以從成品 DB 提取成品。 這是必要的，因為不同的節點可能有不同的成品資料庫，而且節點不知道這一點。 如果第1個節點已關閉，成品解析將會失敗。 由於這種設計，不需要在不同的資料中心之間複寫成品 DB。 如果整個資料中心已關閉，則建立成品的節點很可能也會關閉，這表示無法再解析成品。  

 - **外部網路鎖定** 

    在設定檔中參考的成品資料庫將用於外部網路鎖定資料。 不過，針對 ESL 功能，AD FS 會選擇在成品 DB 中寫入資料的主資料庫。 所有節點都會對主要節點進行 REST API 呼叫，以取得和設定每個使用者的最新資訊。 如果使用多個成品 DB，則系統管理員必須為每個成品 DB 或資料中心選取主要節點。 

    若要選取一個節點做為 ESL 主機，請流覽至 ADFS 節點的設定檔，並在 "IdentityServer" 區段下新增下列內容：       
    
    在 master 上新增下列專案。 請注意，這三個金鑰都區分大小寫。 

    &lt;useractivityfarmrole masterFQDN = [所選主要的 FQDN] isMaster = "true"/&gt;
    
    在其他節點上，新增下列專案：

   &lt;useractivityfarmrole masterFQDN = [所選主要的 FQDN] isMaster = "false"/&gt;
 
    >[!NOTE] 
    >因為多個成品資料庫不會同步處理資料，所以成品 Db 之間的 ESL 值不會同步。
    使用者可能會針對要求叫用不同的資料中心，因此 ExtranetLockoutThreshold 相依于成品 Db 的數目，ExtranetLockoutThreshold * 成品 Db 的數目。 
 
  - **權杖重新執行偵測** 
    
    一律會從中央成品資料庫呼叫權杖重新執行偵測資料。 AD FS 會儲存來自宣告提供者信任的權杖，以確保無法重新執行相同的 token。 如果攻擊者嘗試重新執行相同的權杖，AD FS 會驗證此權杖是否存在於成品 DB 中。 如果權杖存在，則會拒絕要求。 中央成品資料庫用於安全性，因為不會複寫成品 DB 資料，攻擊者可以將要求傳送至另一個資料中心，並重新執行權杖。 在此案例中，建立 ArtifactDB 的其他唯讀複本並不會防止跨資料中心的延遲，因為只會使用中央成品資料庫。    
 
 