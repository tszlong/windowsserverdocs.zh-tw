---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: 為容錯移轉叢集部署雲端見證
ms.prod: windows-server
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 01/18/2019
description: 如何使用 Microsoft Azure 來裝載雲端中 Windows Server 容錯移轉叢集的見證，亦即如何部署雲端見證。
ms.openlocfilehash: ad5ff47a72319fee7650d1d9c0d0616cfaaa22d3
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948170"
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>部署容錯移轉叢集的雲端見證

> 適用于： Windows Server 2019、Windows Server 2016

雲端見證是一種容錯移轉叢集仲裁見證，它會使用 Microsoft Azure 來提供叢集仲裁的投票。 本主題概要說明雲端見證功能、其支援的案例，以及如何為容錯移轉叢集設定雲端見證的指示。

## <a name="CloudWitnessOverview"></a>雲端見證總覽

[圖 1] 說明使用 Windows Server 2016 的多網站延展容錯移轉叢集仲裁設定。 在此範例設定（圖1）中，2個資料中心內有2個節點（稱為「網站」）。 請注意，叢集可能會跨越超過2個資料中心。 此外，每個資料中心都可以有2個以上的節點。 這項安裝程式中的典型叢集仲裁設定（自動容錯移轉 SLA）會提供每個節點的投票。 仲裁見證會提供一項額外的投票，讓叢集可以繼續執行，即使其中一個資料中心發生電源中斷的情況也一樣。 數學運算很簡單-總共有5個投票，您需要3個投票讓叢集保持執行狀態。  

![第三個不同網站中的檔案共用見證，2個其他網站中有2個節點](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "檔案共用見證")  
**圖1：使用檔案共用見證作為仲裁見證**  

在一個資料中心發生電源中斷的情況下，為了讓其他資料中心的叢集有相當的機會讓它保持執行狀態，建議您將仲裁見證裝載于兩個資料中心以外的位置。 這通常表示需要第三個不同的資料中心（網站）來裝載檔案伺服器，而該伺服器會支援用來做為仲裁見證的檔案共用（檔案共用見證）。  

大部分的組織都沒有第三個個別的資料中心，它會主控檔案伺服器來支援檔案共用見證。 這表示組織主要會將檔案伺服器裝載于兩個資料中心的其中一個，而擴充功能會使該資料中心成為主要資料中心。 在主要資料中心發生電源中斷的情況下，叢集會中斷，因為另一個資料中心只會有2個投票，而這是在所需的最高3個投票底下。 對於具有第三個個別資料中心來主控檔案伺服器的客戶，維護支援檔案共用見證的高可用性檔案伺服器會造成額外負荷。 在公用雲端中裝載虛擬機器，其中具有在虛擬作業系統中執行之檔案共用見證的檔案伺服器，在安裝 & 維護方面，會有很大的負擔。  

雲端見證是一種新類型的容錯移轉叢集仲裁見證，其利用 Microsoft Azure 作為仲裁點（圖2）。 它會使用 Azure Blob 儲存體來讀取/寫入 Blob 檔案，而該檔案接著會在分裂式解析時作為仲裁點使用。  

這種方法有很大的好處：
1. 利用 Microsoft Azure （不需要第三個個別的資料中心）。  
2. 使用標準可用的 Azure Blob 儲存體（在公用雲端中裝載的虛擬機器不會有額外的維護負荷）。  
3. 相同的 Azure 儲存體帳戶可以用於多個叢集（每個叢集一個 blob 檔案; 做為 blob 檔案名使用的叢集唯一識別碼）。  
4. 儲存體帳戶的持續 $cost 非常低（每個 blob 檔案寫入的資料非常少，blob 檔案只會在叢集節點的狀態變更時更新一次）。  
5. 內建雲端見證資源類型。  

![圖表說明以雲端見證作為仲裁見證的多網站延展叢集](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**圖2：以雲端見證作為仲裁見證的多網站延展叢集**  

如 [圖 2] 所示，沒有第三個不同的網站。 雲端見證與其他仲裁見證一樣，會取得投票，並可參與仲裁計算。  

## <a name="CloudWitnessSupportedScenarios"></a>雲端見證：單一見證類型的支援案例
如果您有容錯移轉叢集部署，其中所有節點都可以連線到網際網路（透過 Azure 的延伸模組），建議您將雲端見證設定為仲裁見證資源。  

支援使用雲端見證作為仲裁見證的部分案例如下所示：  
- 嚴重損壞修復延伸多網站叢集（請參閱 [圖 2]）。  
- 不含共用存放裝置的容錯移轉叢集（SQL Always On 等）。  
- 在裝載于 Microsoft Azure 虛擬機器角色（或任何其他公用雲端）中的來賓 OS 內執行的容錯移轉叢集。  
- 在裝載于私人雲端之虛擬機器的客體作業系統內執行的容錯移轉叢集。
- 具有或不含共用存放裝置的存放裝置叢集，例如向外延展檔案伺服器叢集。  
- 小型分支-辦公室叢集（甚至2個節點的叢集）  

從 Windows Server 2012 R2 開始，建議一律設定見證，因為叢集會自動管理見證投票，而節點會使用動態仲裁來投票。  

## <a name="CloudWitnessSetUp"></a>設定叢集的雲端見證
若要為您的叢集設定雲端見證作為仲裁見證，請完成下列步驟：
1. 建立用來作為雲端見證的 Azure 儲存體帳戶
2. 將雲端見證設定為叢集的仲裁見證。

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>建立用來作為雲端見證的 Azure 儲存體帳戶

本節說明如何建立儲存體帳戶及查看和複製該帳戶的端點 Url 和存取金鑰。

若要設定雲端見證，您必須擁有有效的 Azure 儲存體帳戶，可用來儲存 blob 檔案（用於仲裁）。 雲端見證會在 Microsoft 儲存體帳戶底下建立知名的容器**msft-雲端見證**。 雲端見證會寫入單一 blob 檔案，其中具有對應的叢集唯一識別碼，做為此**msft-雲端見證**容器下 blob 檔案的檔案名。 這表示您可以使用相同的 Microsoft Azure 儲存體帳戶，為多個不同的叢集設定雲端見證。

當您使用相同的 Azure 儲存體帳戶來為多個不同的叢集設定雲端見證時，會自動建立單一的**msft-雲端見證**容器。 此容器將包含每個叢集的一個 blob 檔案。

### <a name="to-create-an-azure-storage-account"></a>建立 Azure 儲存體帳戶

1. 登入 [Azure 入口網站](https://portal.azure.com)。
2. 在 [集線器] 功能表中，選擇 [新增] -> [資料 + 儲存體] -> [儲存體帳戶]。
3. 在 [建立儲存體帳戶] 頁面中，執行下列動作：
    1. 輸入儲存體帳戶的名稱。
    <br>儲存體帳戶名稱必須介於 3 到 24 個字元的長度，而且只能包含數字和小寫字母。 儲存體帳戶名稱在 Azure 內也必須是唯一的。
        
    2. 針對 [**帳戶類型**]，選取 **[一般用途**]。
    <br>您不能將 Blob 儲存體帳戶用於雲端見證。
    3. 針對 [效能]，請選取 [標準]。
    <br>您無法將 Azure 進階儲存體用於雲端見證。
    2. 針對 **[** 複寫]，選取 **[本地-多餘儲存體（LRS）** ]。
    <br>容錯移轉叢集會使用 blob 檔案作為仲裁點，而在讀取資料時需要一些一致性保證。 因此，您**必須針對複寫**類型選取 [**本機-多餘的儲存體**]。

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>針對您的 Azure 儲存體帳戶，查看並複製儲存體存取金鑰

當您建立 Microsoft Azure 儲存體帳戶時，它會與自動產生的兩個存取金鑰相關聯-主要存取金鑰和次要存取金鑰。 若為第一次建立雲端見證，請使用**主要存取金鑰**。 對於要用於雲端見證的金鑰沒有任何限制。  

#### <a name="to-view-and-copy-storage-access-keys"></a>若要查看並複製儲存體存取金鑰

在 Azure 入口網站中，流覽至您的儲存體帳戶、按一下 [**所有設定**]，然後按一下 [**存取金鑰**] 以查看、複製和重新產生您的帳戶存取金鑰。 [存取金鑰] 分頁也包含預先設定的連接字串，其使用您的主要和次要金鑰，您可以複製以在應用程式中使用（請參閱 [圖 4]）。

Microsoft Azure](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png) 中 [管理存取金鑰] 對話方塊的 ![快照集  
**圖4：儲存體存取金鑰**

### <a name="view-and-copy-endpoint-url-links"></a>查看及複製端點 URL 連結  
當您建立儲存體帳戶時，會使用以下格式來產生下列 Url： `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

雲端見證一律使用**Blob**做為儲存體類型。 Azure 會使用**core.windows.net**作為端點。 設定雲端見證時，您可能會根據您的案例，使用不同的端點來設定它（例如，中國的 Microsoft Azure 資料中心有不同的端點）。  

> [!NOTE]  
> 端點 URL 是由雲端見證資源自動產生的，而且 URL 不需要進行額外的設定步驟。  

#### <a name="to-view-and-copy-endpoint-url-links"></a>若要查看及複製端點 URL 連結
在 Azure 入口網站中，流覽至您的儲存體帳戶、按一下 [**所有設定**]，然後按一下 [**屬性**] 以查看並複製您的端點 url （請參閱 [圖 5]）。  

![雲端見證端點連結的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**圖5：雲端見證端點 URL 連結**

如需建立和管理 Azure 儲存體帳戶的詳細資訊，請參閱[關於 Azure 儲存體帳戶](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>將雲端見證設定為叢集的仲裁見證
雲端見證設定已妥善整合到內建于容錯移轉叢集管理員的現有仲裁設定向導中。  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>將雲端見證設定為仲裁見證
1. 啟動容錯移轉叢集管理員。
2. 以滑鼠右鍵按一下叢集 >**其他動作** -> **設定叢集仲裁設定** （請參閱 圖 6）。 這會啟動 [設定叢集仲裁]。  
    ![功能表路徑的快照，以在容錯移轉叢集管理員 UI 中即可設定叢集仲裁設定](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png) [**圖 6]。叢集仲裁設定**

3. 在 [**選取仲裁**設定] 頁面上，選取 **[選取仲裁見證**] （請參閱 [圖 7]）。  

    在叢集仲裁嚮導中，![[選取 quotrum 見證] 選項按鈕的快照集](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **[圖 7]選取仲裁設定**

4. 在 [**選取仲裁見證**] 頁面上，選取 [**設定雲端見證**] （請參閱 [圖 8]）。  

    ![適當選項按鈕的快照集來選取雲端見證](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **[圖 8]選取仲裁見證**  

5. 在 [**設定雲端見證**] 頁面上，輸入下列資訊：  
   1. （必要參數）Azure 儲存體帳戶名稱。  
   2. （必要參數）對應至儲存體帳戶的存取金鑰。  
       1. 第一次建立時，請使用主要存取金鑰（請參閱 [圖 5]）  
       2. 輪替主要存取金鑰時，請使用次要存取金鑰（請參閱 [圖 5]）  
   3. （選擇性參數）如果您想要使用不同的 Azure 服務端點（例如中國的 Microsoft Azure 服務），請更新端點伺服器名稱。  

      在叢集仲裁嚮導中，![[雲端見證設定] 窗格的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
      **圖9：設定您的雲端見證**

6. 成功設定雲端見證之後，您可以在 [容錯移轉叢集管理員] 嵌入式管理單元中查看新建立的見證資源（請參閱 [圖 10]）。

    ![成功設定雲端見證](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **圖10：成功設定雲端見證**

### <a name="configuring-cloud-witness-using-powershell"></a>使用 PowerShell 設定雲端見證  
現有的 Set-clusterquorum PowerShell 命令有對應至雲端見證的新額外參數。  

您可以使用下列 PowerShell 命令來設定雲端見證[`Set-ClusterQuorum`](https://technet.microsoft.com/library/ee461013.aspx) ：  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

如果您需要使用不同的端點（罕見）：  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>Azure 儲存體雲端見證的帳戶考慮  
將雲端見證設定為容錯移轉叢集的仲裁見證時，請考慮下列事項：
* 您的容錯移轉叢集不會儲存存取金鑰，而是會產生並安全地儲存共用存取安全性（SAS）權杖。  
* 只要存取金鑰仍然有效，所產生的 SAS 權杖就會是有效的。 輪替主要存取金鑰時，請務必先使用次要存取金鑰更新雲端見證（在所有使用該儲存體帳戶的叢集上），然後再重新產生主要存取金鑰。  
* 雲端見證會使用 Azure 儲存體帳戶服務的 HTTPS REST 介面。 這表示它需要在所有叢集節點上開啟 HTTPS 埠。

### <a name="proxy-considerations-with-cloud-witness"></a>雲端見證的 Proxy 考慮  
雲端見證會使用 HTTPS （預設通訊埠443）來建立與 Azure blob 服務的通訊。 確定可以透過網路 Proxy 存取 HTTPS 埠。

## <a name="see-also"></a>請參閱
- [Windows Server 中容錯移轉叢集的新功能](whats-new-in-failover-clustering.md)
