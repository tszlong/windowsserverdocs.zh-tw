---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: 為容錯移轉叢集部署雲端見證
manager: lizross
ms.author: jgerend
ms.topic: article
author: JasonGerend
ms.date: 01/18/2019
description: 如何使用 Microsoft Azure 來裝載雲端中 Windows Server 容錯移轉叢集的見證，也就是如何部署雲端見證。
ms.openlocfilehash: fe613492f290cbcee1ee176fc42b54b4e1f38883
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904353"
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>部署容錯移轉叢集的雲端見證

> 適用於：Windows Server 2019、Windows Server 2016

雲端見證是一種容錯移轉叢集仲裁見證類型，使用 Microsoft Azure 提供叢集仲裁投票功能。 本主題概要說明雲端見證功能、其支援的案例，以及如何設定容錯移轉叢集的雲端見證的指示。

## <a name="cloud-witness-overview"></a><a name="CloudWitnessOverview"></a>雲端見證總覽

[圖 1] 說明 Windows Server 2016 的多重網站延展容錯移轉叢集仲裁設定。 在此範例設定中 (圖 1) ，2個資料中心內有2個節點 (稱為「網站) 」。 請注意，叢集有可能跨越2個以上的資料中心。 此外，每個資料中心都可以有2個以上的節點。 此安裝程式中的典型叢集仲裁設定 (自動容錯移轉 SLA) 為每個節點提供投票。 仲裁見證會提供一個額外的投票，讓叢集即使在其中一個資料中心發生電源中斷時，仍可繼續執行。 數學運算很簡單-總共有5個投票，而您需要3個投票來讓叢集保持運作。

![第三個網站中有2個節點](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "檔案共用見證") 
 的第三個不同網站中的檔案共用見證 **圖1：使用檔案共用見證作為仲裁見證**

如果某個資料中心的電源中斷，為了讓其他資料中心的叢集有相當的機會讓它繼續執行，建議您在兩個資料中心以外的位置裝載仲裁見證。 這通常表示需要第三個不同的資料中心 (網站) 來裝載正在支援檔案共用的檔案伺服器，而該檔案共用會用來作為仲裁見證 (檔案共用見證) 。

大部分的組織都沒有第三個不同的資料中心，可裝載檔案伺服器以支援檔案共用見證。 這表示組織主要是在兩個資料中心（依擴充功能）中裝載檔案伺服器，使該資料中心成為主要資料中心。 在主要資料中心發生電源中斷的情況下，叢集將會關閉，因為另一個資料中心只會有2個投票，其低於所需的3個投票的仲裁。 如果客戶有第三個不同的資料中心來裝載檔案伺服器，則維護支援檔案共用見證的高可用性檔案伺服器會有額外負荷。 在公用雲端中裝載虛擬機器，而在來賓 OS 中執行檔案共用見證的檔案伺服器，在安裝 & 維護方面，會有很大的負擔。

雲端見證是一種新的容錯移轉叢集仲裁見證，其利用 Microsoft Azure 作為仲裁點 ([圖 2]) 。 它會使用 Azure Blob 儲存體來讀取/寫入 Blob 檔案，然後在分裂解析度時作為仲裁點使用。

這種方法有很大的好處：
1. 利用 Microsoft Azure (不需要第三個不同的資料中心) 。
2. 使用標準可用 Azure Blob 儲存體 (公用雲端) 中裝載的虛擬機器沒有額外的維護負擔。
3. 您可以針對多個叢集使用相同的 Azure 儲存體帳戶， (每個叢集一個 blob 檔案;用來作為 blob 檔案名) 的叢集唯一識別碼。
4. 儲存體帳戶的 $cost 非常低， (每個 blob 檔案寫入的極小資料，只要叢集節點的狀態變更) ，blob 檔案就會更新一次。
5. 內建的雲端見證資源類型。

![此圖說明以雲端見證作為仲裁見證的多重網站延展叢集 ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)
 **圖2：以雲端見證作為仲裁見證的多網站延伸** 叢集

如 [圖 2] 所示，不需要第三個不同的網站。 如同其他任何仲裁見證，雲端見證會進行投票，而且可以參與仲裁計算。

## <a name="cloud-witness-supported-scenarios-for-single-witness-type"></a><a name="CloudWitnessSupportedScenarios"></a>雲端見證：單一見證類型的支援案例

如果您有容錯移轉叢集部署，其中所有節點都可以透過 Azure) 的延伸模組連線到網際網路 (，建議您將雲端見證設定為仲裁見證資源。

某些支援使用雲端見證作為仲裁見證的案例如下：
- 嚴重損壞修復延伸的多網站叢集 (請參閱 [圖 2]) 。
- 不含共用儲存體的容錯移轉叢集 (SQL Always On 等 ) 。
- 在裝載于 Microsoft Azure 虛擬機器角色 (或任何其他公用雲端) 的虛擬機器中執行的容錯移轉叢集。
- 在私人雲端中裝載之虛擬機器的客體作業系統內執行的容錯移轉叢集。
- 具有或不具有共用存放裝置的儲存體叢集，例如擴充檔案伺服器叢集。
- 小型分支-辦公室叢集 (甚至是2個節點的叢集) 

從 Windows Server 2012 R2 開始，建議一律設定見證，因為叢集會自動管理見證投票，而節點則會使用動態仲裁來投票。

## <a name="set-up-a-cloud-witness-for-a-cluster"></a><a name="CloudWitnessSetUp"></a> 設定叢集的雲端見證

若要為您的叢集設定雲端見證作為仲裁見證，請完成下列步驟：
1. 建立用來作為雲端見證的 Azure 儲存體帳戶
2. 將雲端見證設定為叢集的仲裁見證。

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>建立用來作為雲端見證的 Azure 儲存體帳戶

本節說明如何建立儲存體帳戶，並查看並複製該帳戶的端點 Url 和存取金鑰。

若要設定雲端見證，您必須具有有效的 Azure 儲存體帳戶，可用來儲存 blob 檔案 (用於仲裁) 。 Cloud 見證會在 Microsoft 儲存體帳戶下建立知名的容器 **msft-雲端見證** 。 雲端見證會寫入單一 blob 檔案，其中包含對應叢集的唯一識別碼，用來作為此 **msft-雲端見證** 容器下的 blob 檔案名。 這表示您可以使用相同的 Microsoft Azure 儲存體帳戶來設定多個不同叢集的雲端見證。

當您使用相同的 Azure 儲存體帳戶來設定多個不同叢集的雲端見證時，系統會自動建立單一的 **msft-雲端見證** 容器。 此容器會在每個叢集包含一個 blob 檔案。

### <a name="to-create-an-azure-storage-account"></a>若要建立 Azure 儲存體帳戶

1. 登入 [Azure 入口網站](https://portal.azure.com)。
2. 在 [集線器] 功能表中，選擇 [新增] -> [資料 + 儲存體] -> [儲存體帳戶]。
3. 在 [建立儲存體帳戶] 頁面中，執行下列動作：
    1. 輸入儲存體帳戶的名稱。
    <br>儲存體帳戶名稱必須介於 3 到 24 個字元的長度，而且只能包含數字和小寫字母。 儲存體帳戶名稱在 Azure 內也必須是唯一的。

    2. 針對 [ **帳戶類型**]，選取 **[一般用途**]。
    <br>您無法將 Blob 儲存體帳戶用於雲端見證。
    3. 針對 [效能]，請選取 [標準]。
    <br>您無法將 Azure 進階儲存體用於雲端見證。
    2. 針對 **複寫**，請選取 [ **本機-多餘的儲存體] (LRS)** 。
    <br>「容錯移轉叢集」會使用 blob 檔案作為仲裁點，在讀取資料時需要一些一致性保證。 因此，您 **必須針對複寫** 類型選取 **本機-多餘的儲存體**。

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>查看並複製您 Azure 儲存體帳戶的儲存體存取金鑰

當您建立 Microsoft Azure 儲存體帳戶時，它會與自動產生的兩個存取金鑰（主要存取金鑰和次要存取金鑰）相關聯。 如果是第一次建立雲端見證，請使用 **主要存取金鑰**。 對於要用於雲端見證的金鑰，沒有任何相關限制。

#### <a name="to-view-and-copy-storage-access-keys"></a>若要查看及複製儲存體存取金鑰

在 Azure 入口網站中，流覽至您的儲存體帳戶，按一下 [ **所有設定** ]，然後按一下 [ **存取金鑰** ]，以查看、複製及重新產生您的帳戶存取金鑰。 存取金鑰分頁也包含預先設定的連接字串，其使用您可以複製以在應用程式中使用的主要和次要金鑰 (請參閱 [圖 4]) 。

![[管理存取金鑰] 對話方塊的快照 Microsoft Azure ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)
 **圖4：儲存體存取金鑰**

### <a name="view-and-copy-endpoint-url-links"></a>查看和複製端點 URL 連結

當您建立儲存體帳戶時，會使用下列格式產生下列 Url： `https://<Storage Account Name>.<Storage Type>.<Endpoint>`

雲端見證一律使用 **Blob** 作為儲存體類型。 Azure 會使用 **core.windows.net** 作為端點。 設定雲端見證時，您可能會根據您的案例使用不同的端點進行設定 (例如，中國的 Microsoft Azure datacenter 有不同的端點) 。

> [!NOTE]
> 端點 URL 是由雲端見證資源自動產生的，而且不需要額外的 URL 設定步驟。

#### <a name="to-view-and-copy-endpoint-url-links"></a>若要查看並複製端點 URL 連結

在 Azure 入口網站中，流覽至您的儲存體帳戶，按一下 [ **所有設定** ]，然後按一下 [ **屬性** ] 以查看並複製您的端點 url (請參閱 [圖 5]) 。

![雲端見證端點的快照連結 ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)
 **圖5：雲端見證端點 URL 連結**

如需有關建立和管理 Azure 儲存體帳戶的詳細資訊，請參閱 [關於 Azure 儲存體帳戶](/azure/storage/common/storage-account-create)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>將雲端見證設定為叢集的仲裁見證

雲端見證設定在容錯移轉叢集管理員內建的現有仲裁設定 Wizard 內已妥善整合。

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>設定雲端見證作為仲裁見證

1. 啟動容錯移轉叢集管理員。

2. 以滑鼠右鍵按一下叢集->**更多動作**  ->  **設定叢集仲裁設定** (請參閱圖 6) 。 這會啟動「設定叢集仲裁嚮導」。

    ![在容錯移轉叢集管理員 UI 圖6中，用來設定叢集仲裁設定之功能表路徑的快照 ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png) **。叢集仲裁設定**

3. 在 [ **選取仲裁** 設定] 頁面上，選取 **[選取仲裁見證** (查看 [圖 7]) 。

    ![叢集仲裁嚮導 [圖 7] 中 [選取仲裁見證] 選項按鈕的快照 ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png) **。選取仲裁** 設定

4. 在 [ **選取仲裁見證** ] 頁面上，選取 [ **設定雲端見證** (查看 [圖 8]) 。

    ![適當選項按鈕的快照集，以選取雲端見證 ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png) **圖8。選取仲裁見證**

5. 在 [ **設定雲端見證** ] 頁面上，輸入下列資訊：
   1. Azure 儲存體帳戶名稱)  (必要參數。
   2.  (必要參數) 存取金鑰組應至儲存體帳戶。
       1. 第一次建立時，請使用主要存取金鑰 (請參閱 [圖 5]) 
       2. 輪替主要存取金鑰時，請使用次要存取金鑰 (請參閱 [圖 5]) 
   3.  (選擇性參數) 如果您打算使用不同的 Azure 服務端點 (例如中國) 的 Microsoft Azure 服務，則更新端點伺服器名稱。

      ![叢集仲裁 wizard 中的 [雲端見證設定] 窗格的快照 ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)
       **圖9：設定您的雲端見證**

6. 成功設定雲端見證之後，您可以在容錯移轉叢集管理員嵌入式管理單元中查看新建立的見證資源 (請參閱 [圖 10]) 。

    ![成功設定 Cloud 見證 ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png) **圖10：成功設定雲端見證**

### <a name="configuring-cloud-witness-using-powershell"></a>使用 PowerShell 設定雲端見證

現有的 Set-ClusterQuorum PowerShell 命令具有對應至雲端見證的新額外參數。

您可以 [`Set-ClusterQuorum`](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum) 使用下列 PowerShell 命令，以 Cmdlet 設定 Cloud 見證：

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

如果您需要使用不同的端點 (罕見的) ：

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>雲端見證 Azure 儲存體帳戶考慮

將雲端見證設定為容錯移轉叢集的仲裁見證時，請考慮下列事項：
- 您的容錯移轉叢集會產生並安全地將共用存取安全性儲存 (SAS) 權杖，而不是儲存存取金鑰。
- 只要存取金鑰仍有效，產生的 SAS 權杖就會有效。 輪替主要存取金鑰時，請務必先更新所有使用該儲存體帳戶的叢集上的雲端見證 (，) 搭配次要存取金鑰，然後再重新產生主要存取金鑰。
- 雲端見證使用 Azure 儲存體帳戶服務的 HTTPS REST 介面。 這表示它需要在所有叢集節點上開啟 HTTPS 埠。

### <a name="proxy-considerations-with-cloud-witness"></a>雲端見證的 Proxy 考慮

雲端見證使用 HTTPS (預設埠 443) 來建立與 Azure blob 服務的通訊。 確定可以透過網路 Proxy 存取 HTTPS 埠。

## <a name="see-also"></a>另請參閱

- [Windows Server 容錯移轉叢集的新功能](whats-new-in-failover-clustering.md)
