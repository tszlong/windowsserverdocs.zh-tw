---
title: Fine Tuning SQL 和定址與 AD FS 的延遲問題
description: 本文討論如何微調 SQL 搭配 AD FS。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 06/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb699a1f92013f5657d2fbb48b203f96a5e5a5ba
ms.sourcegitcommit: 6b6c3601fb7493ab145ccff02db26d7123df9a3d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322854"
---
# <a name="fine-tuning-sql-and-addressing-latency-issues-with-ad-fs"></a>Fine tuning SQL 和定址與 AD FS 的延遲問題
中的更新[AD FS 2016](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294)我們引進了下列改善，以減少跨資料庫的延遲。 適用於 AD FS 2019 的未來更新會包含這些增強功能。

## <a name="in-memory-cache-update-in-background-thread"></a>在背景執行緒中的記憶體中快取更新 
在先前 Alwayson 可用性 (AoA) 部署的詳細資訊，延遲存在於任何 「 讀取 」 作業因為主要節點可能位於不同的資料中心。 兩個不同的資料中心之間的呼叫會導致延遲。  

在最新更新 AD fs 中，目標是減少延遲，透過背景執行緒，以重新整理 AD FS 設定快取和設定，以可設定重新整理的時間間隔加入。 對於資料庫查閱所花費的時間會大幅減少在要求的執行緒中，因為資料庫快取更新則會移到背景執行緒。  

當`backgroundCacheRefreshEnabled`設為 true，AD FS 可讓背景執行緒來執行快取更新。 從快取中擷取資料的頻率可以藉由設定自訂成時間值`cacheRefreshIntervalSecs`。 預設值設為 300 秒時`backgroundCacheRefreshEnabled`設為 true。 集合值持續時間之後, AD FS 可讓您開始重新整理它的快取，並在進行更新時，舊的快取資料將會繼續使用。  

>[!NOTE]
> 快取的資料會重新整理外部`cacheRefreshIntervalSecs`值如果 ADFS 收到來自 SQL 表示在資料庫中已發生變更的通知。 此通知將會觸發重新整理快取。 

### <a name="recommendations-for-setting-the-cache-refresh"></a>設定快取重新整理建議 
快取重新整理的預設值是**五分鐘**。 建議將它設定為**1 小時**降低 AD fs 不必要的資料重新整理，因為如果發生任何 SQL 變更快取資料將會重新整理。  

AD FS 註冊回呼以 SQL 變更和 ADFS 會發生的變更時收到通知。 透過這種方法，ADFS 會從 SQL 接收每個新的變更，它發生時立即收到。 

發生網路故障而導致 AD FS 遺失 SQL 通知中，AD FS 會快取所指定的間隔上重新整理時重新整理的值。 如果 AD FS 與 SQL 之間疑似任何連線問題，建議將快取重新整理值設為低於 1 小時。  

### <a name="configuration-instructions"></a>組態指示 
組態檔支援多個快取項目。 下面所列的下列所有可設定依據您組織的需求。 

下列範例會啟用背景快取重新整理，並設定為 1800 秒，或 30 分鐘，才會進行快取重新整理間隔。 這必須在 ADFS 中的每個節點上完成，而且必須之後重新啟動 ADFS 服務。 所做的變更不要影響到其他節點並在所有節點中進行變更之前，先測試的第一個節點。 

  1. 瀏覽至 [AD FS 設定檔，並 「 Microsoft.IdentityServer.Service"] 區段下，新增下列項目：  
  
  - `backgroundCacheRefreshEnabled`  -指定是否已啟用背景快取功能。 「 true/false"值。
  - `cacheRefreshIntervalSecs` -以秒為單位的 ADFS 會重新整理快取的值。 如果有任何變更，在 SQL 中的，AD FS 會重新整理快取。 AD FS 會收到 SQL 通知，並重新整理快取。  
 
 >[!NOTE]
 > 在組態檔中的所有項目皆區分大小寫。  
 &lt;cache cacheRefreshIntervalSecs="1800" backgroundCacheRefreshEnabled="true" /&gt; 
 
其他支援的可設定的值： 

   - **maxRelyingPartyEntries** -最大的 AD FS 會保留在記憶體中信賴憑證者合作對象項目數目。 此值也會使用 oAuth 應用程式權限快取。 如果沒有超出每秒要求數量，而且如果所有將儲存在記憶體中的應用程式權限，此值應該是應用程式權限的數目。 預設值為 1000年。
   - **maxIdentityProviderEntries** -此 AD FS 會保留在記憶體中的提供者項目是宣告的最大數目。 預設值為 200。 
   - **maxClientEntries** -這是 AD FS 會保留在記憶體中的 OAuth 用戶端項目數目上限。 預設值為 500。 
   - **maxClaimDescriptorEntries** -最大的 AD FS 會保留在記憶體中的宣告描述元項目數目。 預設值為 500。 
   - **maxNullEntries** -這用來作為負快取。 AD FS 會尋找資料庫中的項目，它找不到 AD FS 會在新增負快取。 這是該快取的大小上限。 負快取，每種類型的物件，它不是單一的快取的所有物件。 預設值是 50,0000。 

## <a name="multiple-artifact-db-support-across-datacenters"></a>多個成品 DB 支援跨資料中心 
針對先前的多個資料中心的設定，AD FS 才支援單一成品資料庫時，擷取呼叫期間造成跨中央資料中心的延遲。  

若要減少跨資料中心的延遲，AD FS 系統管理員可以立即部署多個成品 DB 執行個體，，然後修改 AD FS 節點的組態檔，以指向不同成品 DB 執行個體。 成品的資料庫連接字串可以讓每個節點成品 DB 的組態檔中提供。 如果連接字串不存在的組態檔內，節點會切換回先前的設計，以使用成品資料庫也就是出現在組態資料庫。  
使用此設定也支援混合式環境。  

### <a name="requirements"></a>需求： 
之前設定多個成品資料庫支援，所有節點上執行更新，並更新二進位檔案，因為多節點呼叫會透過這項功能。 
  1. 產生部署指令碼來建立成品 DB:若要部署多個成品 DB 執行個體，系統管理員必須產生成品 db 的 SQL 部署指令碼。 這項更新，現有的一部分`Export-AdfsDeploymentSQLScript`cmdlet 已更新才會選擇性地指定 AD FS 資料庫產生的 SQL 部署指令碼參數中。 
 
 例如，若要只成品 db 產生部署指令碼，請指定`-DatabaseType`參數，並傳入值 「 成品 」。 選擇性`-DatabaseType`參數指定的 AD FS 資料庫型別，並可以設定為：全部 （預設值），成品或組態。 如果沒有`-DatabaseType`參數指定，則指令碼會設定 成品 和 設定指令碼。  

   ```PowerShell
   PS C:\> Export-AdfsDeploymentSQLScript -DestinationFolder <script folder where scripts will be created> -ServiceAccountName <domain\serviceaccount> -DatabaseType "Artifact" 
   ```
產生的指令碼應該建立所需的資料庫並讓 AD FS 服務帳戶，這些資料庫的 SQL SA 權限的 SQL 電腦上執行。

 2. 建立使用部署指令碼的成品 DB。 新產生的 CreateDB.sql 和 SetPermissions.sql 部署指令碼複製到 SQL server 機器並執行它們來建立本機的成品 DB。 
 
 3. 修改組態檔，以加入的成品 DB 連接。 
 瀏覽至 [AD FS 節點的組態檔中，「 Microsoft.IdentityServer.Service"] 區段下，加入新設定 ArtifactDB 的進入點。 

 >[!NOTE] 
 > artifactStore 與 connectionString 則區分大小寫的值。 請確定已正確設定。 &lt;artifactStore connectionString="Data Source=.\SQLInstance;Integrated Security=True;Initial Catalog=AdfsArtifactStore" /&gt; 
>
>使用符合您的 sql 連接的資料來源值。



 4. 重新啟動 AD FS 服務，變更才會生效。 
 
 >[!NOTE] 
 > 不建議使用 SQL 複寫或資料庫成品之間的同步處理。 若要設定每個資料中心的一個成品資料庫建議。 
 
### <a name="cross-datacenter-failover-and-database-recovery"></a>跨資料中心容錯移轉和資料庫復原  
建議您在相同的資料中心做為主要成品資料庫上建立容錯移轉成品資料庫。 發生容錯移轉時，會有任何增加的延遲。 不建議在容錯移轉成品資料庫跨越資料中心。 以下將詳細說明如何呼叫 OAuth、 SAML、 ESL 和權杖重新執行偵測函式具有多個成品的資料庫。 
 - **OAuth 和 SAML** 

   OAuth 和 SAML 成品的要求，節點會建立成品成品 DB 中的組態檔中。 如果組態檔不包含成品的資料庫連接，它會使用常見的成品 DB。 當要擷取之成品的下一個要求進入至另一個節點時，另一個節點會對第 1 個節點，以擷取成品 DB 中的成品的 rest API。 不同的節點可能會有不同的成品資料庫以及節點並不知道的這是必要的。 如果第 1 個節點已關閉，成品解析將會失敗。 基於這項設計，複寫在不同的資料中心的成品 DB 不需要。 如果一整個資料中心已關閉，最有可能建立成品的節點也關閉，這表示不能再解析該成品。  

 - **外部網路鎖定** 

    在組態檔中參考的成品資料庫將用於外部網路鎖定的資料。 不過，ESL 功能中，AD FS 會選擇將資料寫入成品 DB 中的主機。 所有節點都進行 REST API 呼叫來取得及設定最新每個使用者的相關資訊的主要節點。 如果多個成品 DB 的使用中，系統管理員必須選取每個成品 DB 或資料中心的主要節點。 

    若要選取一個節點是 ESL 主機，請瀏覽至 ADFS 節點的組態檔中，然後 「 Microsoft.IdentityServer.Service" 區段下，新增下列內容：       
    
    在主機上新增下列項目。 請注意，所有的三個索引鍵是區分大小寫。 

    &lt;useractivityfarmrole masterFQDN = 選取的主要 FQDN Ismaster="0 ="true"/&gt;
    
    在其他節點上新增下列項目：

   &lt;useractivityfarmrole masterFQDN = 選取的主要 FQDN Ismaster="0 ="false"/&gt;
 
    >[!NOTE] 
    >多個成品資料庫不會同步處理資料，因為 ESL 值將不會同步處理成品資料庫之間。
    使用者可以可能叫用不同的資料中心，是要求，因此讓 ExtranetLockoutThreshold 相依的成品 Db 及 ExtranetLockoutThreshold 數目 * 成品資料庫數目。 
 
  - **權杖重新執行偵測** 
    
    權杖重新執行偵測資料一律會從中央資料庫中，成品呼叫。 AD FS 將權杖儲存從宣告提供者信任，確保無法重新執行相同的語彙基元。 如果攻擊者嘗試重新執行相同的語彙基元時，AD FS 會驗證權杖是否在成品 DB。 如果權杖存在時，將會拒絕要求。 使用中央資料庫中，成品的安全性，因為成品 DB 資料不在複寫中，攻擊者可能將要求傳送至另一個資料中心並重新執行權杖。 建立額外的唯讀複本的 ArtifactDB 在中央成品資料庫使用時，不會防止跨資料中心的延遲，在此案例中。    
 
 