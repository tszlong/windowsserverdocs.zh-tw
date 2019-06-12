---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: 部署容錯移轉叢集的雲端見證
ms.prod: windows-server-threshold
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 01/18/2019
description: 如何使用 Microsoft Azure 來裝載在雲端中的 Windows Server 容錯移轉叢集的見證如何也稱為部署雲端見證。
ms.openlocfilehash: 64fd39a37c63d24f8fc0eb4f45c8a7e9f6089013
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439778"
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>部署容錯移轉叢集的雲端見證

> 適用於：Windows Server 2019，Windows Server 2016

雲端見證是一種容錯移轉叢集 」 仲裁見證，使用 Microsoft Azure 提供在叢集中的仲裁投票。 本主題提供 「 雲端見證 」 功能，其支援的案例和指示如何設定容錯移轉叢集的雲端見證的概觀。

## <a name="CloudWitnessOverview"></a>雲端見證概觀

[圖 1] 說明多站台延伸容錯移轉叢集仲裁設定與 Windows Server 2016。 在此範例組態中 （圖 1），2 個節點中有 2 個資料中心 （又稱為站台）。 請注意，就可以跨越多個 2 個資料中心叢集。 此外，每個資料中心可以有 2 個以上的節點。 在此設定中 （自動容錯移轉 SLA） 的典型的叢集仲裁設定可讓每個節點的投票。 其中一個額外的投票給仲裁見證來允許將執行即使任一個的其中一個資料中心的叢集發生電源中斷。 數學很簡單-有 5 個票數總計，您需要 3 個投票的叢集，以讓它保持執行。  

![在第三個不同的檔案共用見證的站台 2 中的 2 個節點與其他站台](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "檔案共用見證")  
**圖 1:使用檔案共用見證仲裁見證**  

如果發生電源中斷，在一個資料中心，提供機會在其他資料中心，以讓它保持執行，叢集的建議來裝載兩個資料中心以外的位置中的仲裁見證。 這通常表示需要第三個不同的資料中心 （網站） 以裝載檔案伺服器支援檔案共用，會做為仲裁見證 （檔案共用見證）。  

大部分的組織不需要第三個不同的資料中心將裝載檔案伺服器備份檔案共用見證。 這表示組織主要裝載檔案伺服器，其中兩個資料中心的延伸模組，讓該資料中心的主要資料中心。 在案例中為其他資料中心才會有 2 個投票低於所需的 3 個投票的仲裁多數當主要資料中心發生電源中斷，叢集會當機。 有第三個不同的資料中心，以裝載檔案伺服器的客戶，是維護備份檔案共用見證的高可用性檔案伺服器的額外負荷。 裝載公用雲端中的虛擬機器的客體 OS 中執行的檔案共用見證具有檔案伺服器是安裝與維護方面的重大額外負荷。  

雲端見證是新類型的容錯移轉叢集 」 仲裁見證，運用 Microsoft Azure 作為仲裁點 （圖 2）。 它會使用 Azure Blob 儲存體讀取/寫入的 blob 檔案，然後用作為仲裁點發生裂腦解析時。  

有重要權益的這種方法：
1. 利用 Microsoft Azure （不需要第三個個別的資料中心）。  
2. 使用標準可以使用 Azure Blob 儲存體 （裝載於公用雲端中的虛擬機器沒有額外的維護負擔）。  
3. 相同的 Azure 儲存體帳戶可以用於多個群集 （一個 blob 檔案，每個叢集，叢集的唯一識別碼做為 blob 檔案名稱）。  
4. 儲存體帳戶 （非常小的資料寫入每個 blob 檔案，當叢集節點的狀態變更時，只要更新 blob 檔案） 的持續 $cost 非常低。  
5. 內建的雲端見證資源類型。  

![說明多站台延伸叢集中為仲裁見證的雲端見證的圖表](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**圖 2:多站台延伸的叢集為仲裁見證的雲端見證**  

圖 2 所示，沒有任何第三個不同的站台所需。 雲端見證，像任何其他仲裁見證，會獲得票數，並可以參與仲裁計算。  

## <a name="CloudWitnessSupportedScenarios"></a>雲端見證：單一見證類型的支援的案例
如果您有一種容錯移轉叢集部署，其中所有的節點可以連到網際網路 （藉由 Azure 的延伸模組），建議您將雲端見證設定為仲裁見證資源。  

一些支援的案例使用的雲端見證，因為仲裁見證，如下所示：  
- 災害復原自動縮放 （請參閱 圖 2） 的多網站叢集。  
- 容錯移轉叢集，而不使用共用存放裝置 （SQL 一律在等等）。  
- 裝載於 Microsoft Azure 虛擬機器角色 （或任何其他公用雲端） 的客體 OS 內執行的容錯移轉叢集。  
- 在客體 OS 的虛擬機器執行裝載於私人雲端中的容錯移轉叢集。
- 儲存體叢集包含或不含共用存放裝置，例如向外延展檔案伺服器叢集。  
- 小型的分公司叢集 （甚至是 2 個節點的叢集）  

從 Windows Server 2012 R2 開始，建議一律設定見證，會自動管理叢集的見證投票，並使用動態仲裁投票的節點。  

## <a name="CloudWitnessSetUp"></a> 設定叢集的雲端見證
若要設定雲端見證，為您的叢集仲裁見證，請完成下列步驟：
1. 建立 Azure 儲存體帳戶來做為雲端見證
2. 雲端見證設定為您叢集的仲裁見證。

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>建立 Azure 儲存體帳戶來做為雲端見證

本節說明如何建立儲存體帳戶以及檢視和複製端點 Url 和該帳戶的存取金鑰。

若要設定雲端見證，您必須使用有效的 Azure 儲存體帳戶可用來儲存 blob 檔案 （用於仲裁）。 雲端見證會建立一個已知的容器**msft 雲端見證**Microsoft 儲存體帳戶。 雲端見證寫入對應的單一 blob 檔案叢集的唯一識別碼做為檔案名稱的 blob 檔案，在這**msft 雲端見證**容器。 這表示您可以設定雲端見證的多個不同的叢集使用相同的 Microsoft Azure 儲存體帳戶。

當您使用相同的 Azure 儲存體帳戶進行設定之多個不同的雲端見證叢集，單一**msft 雲端見證**取得自動建立容器。 此容器會包含每個叢集的其中一個 blob 檔案。

### <a name="to-create-an-azure-storage-account"></a>若要建立 Azure 儲存體帳戶

1. 登入[Azure 入口網站](http://portal.azure.com)。
2. 在 [中樞] 功能表中，選取 [新增-> 資料 + 儲存體]-> [儲存體帳戶]。
3. 在 [建立儲存體帳戶] 頁面中，執行下列作業：
    1. 輸入您的儲存體帳戶的名稱。
    <br>儲存體帳戶名稱必須是長度為 3 到 24 個字元之間，而且可能包含數字和小寫字母只。 儲存體帳戶名稱也必須在 Azure 中獨一無二。
        
    2. 針對**帳戶種類**，選取**一般用途**。
    <br>您無法使用 Blob 儲存體帳戶的雲端見證。
    3. 針對**效能**，選取**標準**。
    <br>您無法使用 Azure 進階儲存體，適用於雲端見證。
    2. 針對**複寫**，選取**本地備援儲存體 (LRS)** 。
    <br>容錯移轉叢集，則會作為仲裁點，而這需要一些一致性保證讀取資料時使用的 blob 檔案。 因此您必須選取**本地備援儲存體**for**複寫**型別。

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>Azure 儲存體帳戶的檢視和複製儲存體存取金鑰

當您建立 Microsoft Azure 儲存體帳戶時，它是兩個存取金鑰，會自動產生的主要存取金鑰和次要存取金鑰相關聯。 第一次建立雲端見證時，使用**主要存取金鑰**。 沒有任何限制有關適用於雲端見證使用的金鑰。  

#### <a name="to-view-and-copy-storage-access-keys"></a>若要檢視並複製儲存體存取金鑰

在 Azure 入口網站中，瀏覽至您的儲存體帳戶，請按一下**所有設定**，然後按一下**便捷鍵**檢視、 複製和重新產生帳戶存取金鑰。 存取金鑰 刀鋒視窗也包含您使用的主要和次要金鑰，您可以複製使用 （請參閱 圖 4） 的應用程式中的預先設定的連接字串。

![在 Microsoft Azure 中的 [管理存取金鑰] 對話方塊的快照集](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)  
**圖 4:儲存體存取金鑰**

### <a name="view-and-copy-endpoint-url-links"></a>檢視及複製端點 URL 連結  
當您建立儲存體帳戶時，會產生下列 Url，使用格式： `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

一律會使用雲端見證**Blob**作為儲存類型。 Azure 會使用 **。 core.windows.net**做為端點。 在設定雲端見證時，就可以設定它使用不同的端點根據您的案例 （例如中國的 Microsoft Azure 資料中心有不同的端點）。  

> [!NOTE]  
> 端點 URL 會自動產生雲端見證資源，而且沒有任何額外的步驟，設定的所需的 URL。  

#### <a name="to-view-and-copy-endpoint-url-links"></a>若要檢視及複製端點 URL 連結
在 Azure 入口網站中，瀏覽至您的儲存體帳戶，請按一下**所有設定**，然後按一下**屬性**來檢視和複製端點 Url （請參閱 圖 5）。  

![雲端見證端點連結的快照集](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**圖 5:雲端見證端點的 URL 連結**

如需有關建立和管理 Azure 儲存體帳戶的詳細資訊，請參閱[關於 Azure 儲存體帳戶](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>雲端見證設定為您叢集的仲裁見證
完美整合的現有內建到容錯移轉叢集管理員中的仲裁設定精靈 內雲端見證設定。  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>若要設定為仲裁見證的雲端見證
1. 啟動 [容錯移轉叢集管理員]。
2. 以滑鼠右鍵按一下叢集-> **其他動作** -> **設定叢集仲裁設定**（請參閱 圖 6）。 這會啟動 [設定叢集仲裁精靈]。  
    ![設定叢集仲裁設定，在容錯移轉叢集管理員 UI 中的功能表路徑的快照集](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png) **圖 6。叢集仲裁設定**

3. 在 **選取仲裁設定**頁面上，選取**選取仲裁見證**（請參閱 圖 7）。  

    ![快照集的 'select quotrum 見證' 選項在叢集仲裁精靈 的按鈕](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **圖 7。選取仲裁設定**

4. 在 **選取仲裁見證**頁面上，選取**設定的雲端見證**（請參閱 圖 8）。  

    ![適當的選項按鈕以選取的雲端見證的快照集](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **圖 8。選取仲裁見證**  

5. 在 **設定雲端見證**頁面上，輸入下列資訊：  
   1. （也就是必要的參數）Azure 儲存體帳戶名稱。  
   2. （也就是必要的參數）對應至儲存體帳戶存取金鑰。  
       1. 在第一次建立時，使用主要存取金鑰 （請參閱 圖 5）  
       2. 當輪替主要存取金鑰，使用次要存取金鑰 （請參閱 圖 5）  
   3. （選擇性參數）如果您想要使用不同的 Azure 服務端點 （例如中國的 Microsoft Azure 服務），然後更新端點的伺服器名稱。  

      ![在 [叢集仲裁精靈] 的 [雲端見證組態] 窗格的快照集](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
      **圖 9:設定您的雲端見證**

6. 在成功的雲端見證組態，您可以檢視新建立的見證資源在容錯移轉叢集管理員嵌入式管理單元 （請參閱 圖 10）。

    ![成功設定組態的雲端見證](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **圖 10:成功設定組態的雲端見證**

### <a name="configuring-cloud-witness-using-powershell"></a>使用 PowerShell 設定的雲端見證  
現有的 Set-clusterquorum PowerShell 命令會有新額外的參數對應至雲端見證。  

您可以設定使用雲端見證[ `Set-ClusterQuorum` ](https://technet.microsoft.com/library/ee461013.aspx)下列 PowerShell 命令：  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

如果您需要使用不同的端點 （罕見）：  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>雲端見證使用的 azure 儲存體帳戶考量  
當設定為容錯移轉叢集仲裁見證的雲端見證，請考慮下列各項：
* 而不是儲存存取金鑰，會產生您的容錯移轉叢集，並將其安全地儲存的共用存取安全性 (SAS) 權杖中。  
* 所產生的 SAS 權杖是有效的只要存取金鑰會維持有效狀態。 當輪替主要存取金鑰，務必先與次要存取金鑰更新雲端見證 （所有您在叢集上使用該儲存體帳戶），然後再重新產生主要存取金鑰。  
* 雲端見證使用 Azure 儲存體帳戶服務的 HTTPS REST 的介面。 這表示它需要開啟所有叢集節點上的 HTTPS 連接埠。

### <a name="proxy-considerations-with-cloud-witness"></a>Proxy 考量雲端見證  
雲端見證會使用 HTTPS （預設通訊埠 443） 來建立與 Azure blob 服務的通訊。 請確定 HTTPS 連接埠是可透過網路 Proxy 存取。

## <a name="see-also"></a>另請參閱
- [容錯移轉叢集的 Windows Server 最新消息](whats-new-in-failover-clustering.md)
