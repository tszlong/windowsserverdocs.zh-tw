---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: "部署雲端見證容錯移轉叢集"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 10/2/2017
description: "如何使用 Microsoft Azure 裝載的雲端式 Windows Server 容錯移轉叢集見證如何亦部署雲端見證。"
ms.openlocfilehash: 564c6668fcc80a8bd1531c05c142996689de8154
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>部署雲端見證容錯移轉叢集

> 適用於：適用於：Windows Server（以每年次通道）、Windows Server 2016

雲端見證是一種全新的 Windows Server 2016 引進容錯移轉叢集仲裁見證。 本主題提供雲端見證功能、案例的支援，以及了解如何設定執行 Windows Server 2016 容錯移轉叢集雲端見證指示的概觀。

## <a name="CloudWitnessOverview"></a>雲端見證概觀

圖 1 所示多網站延伸容錯移轉叢集仲裁設定與 Windows Server 2016。 在此範例中設定（圖 1），有 2 節點 2 資料中心（稱為網站）。 請注意，就可以跨多個 2 資料中心叢集。 此外，每個 datacenter 可以有超過 2 節點。 此設定（自動容錯移轉 SLA）中的一般叢集仲裁設定可讓每個節點投票。 其中一個額外投票給仲裁見證允許執行即使是一個 datacenter 叢集體驗電源中斷。 很簡單的數學-有 5 總投票，您需要 3 投票給叢集讓它執行。  

![在第三個不同的檔案共用見證網站 2 節點與其他網站](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "檔案共用見證")  
**圖 1: 仲裁見證使用檔案共用見證**  

在停電一個 datacenter 中，叢集其他資料，讓它執行的是，中心提供等機會建議裝載仲裁見證兩個 datacenter 以外的地方。 這通常表示要求第三個不同的 datacenter（網站）主控檔案伺服器備份檔案共用會做為仲裁見證（共用見證檔案）。  

大部分的組織不需要第三個不同的 datacenter 裝載備份檔案共用見證檔案伺服器。 這表示組織主要主機檔案伺服器的兩個資料中心的擴充功能，讓該 datacenter 主要資料中心。 在案例中為其他 datacenter 將只會有 2 投票低於仲裁大部分的 3 投票所需的其中主要 datacenter 中停電，叢集想當機。 第三個不同的資料中心主控檔案伺服器已針對，對於維護備份檔案共用見證可用性檔案伺服器的費用。 主控檔案共用見證執行客體 OS 中有檔案伺服器的虛擬電腦公用雲端是同時設定和維護重要的負擔。  

雲端見證是新的容錯移轉叢集仲裁見證做為（2 圖）仲裁點使用 Microsoft Azure 的類型。 使用 Azure Blob 儲存空間來讀取/寫入可供您在 split-brain 解析度仲裁點大型物件檔案。  

還有重大好處，這種方式：
1. 使用 Microsoft Azure（不需要的第三個不同的 datacenter）。  
2. 使用標準可用 Azure Blob 儲存（虛擬裝載公開雲端中的電腦不額外維護費用）。  
3. Azure 相同的儲存空間帳號可用於多個叢集（每個叢集的一個大型物件檔案; 叢集唯一 id 做為 blob 檔案名稱）。  
4. 極低持續 $cost 儲存過去（每個大型物件檔案，撰寫大型物件檔案更新一次叢集節點狀態時變更非常小資料）。  
5. 建雲端見證資源類型。  

![示範多網站延伸的叢集雲端見證仲裁見證為的簡圖](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**圖 2 所示：多網站延伸雲端見證的叢集為仲裁見證**  

如圖 2 所示，還有所需的任何第三個不同網站。 雲端見證，例如其他仲裁見證取得投票和參與仲裁計算。  

## <a name="CloudWitnessSupportedScenarios"></a>雲端見證：單一見證類型支援案例
如果您擁有所有節點位置（的擴充功能的 Azure）都連接網際網路容錯移轉叢集部署，，建議您將雲端見證設定為您仲裁見證資源。  

一些案例的支援使用雲端見證為仲裁見證如下所示：  
- 損壞修復延展多網站叢集（看到圖 2）。  
- 容錯不共用存放區（SQL 永遠上等）。  
- Microsoft Azure 一樣角色（或任何其他公用雲端）裝載的客體 OS 中執行容錯。  
- 執行中客體 OS 的虛擬機器裝載私人直上雲霄容錯。
- 儲存空間叢集使用或不共用存放裝置，例如延展檔案伺服器叢集。  
- 小分公司叢集（即使是節點 2 叢集）  

開始使用 Windows Server 2012 R2，最好是叢集會自動管理見證投票並節點投票給動態仲裁與設定見證。  

## <a name="CloudWitnessSetUp"></a>用於叢集雲端見證設定
為您叢集仲裁見證設定雲端見證，請完成下列步驟：
1. 建立儲存空間作為雲端見證 Azure 帳號
2. 為您叢集仲裁見證設定雲端見證。

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>建立儲存空間作為雲端見證 Azure 帳號

本章節告訴您如何建立儲存空間 account 檢視及複製端點 Url 和存取帳號該按鍵。

若要設定雲端見證，您必須有效的 Azure 儲存帳號，可以用來儲存（用於仲裁）大型物件檔案。 雲端見證建立已知容器**msft-雲端式見證**底下 Microsoft 儲存體 Account。 雲端見證寫入叢集對應的單一的大型物件檔案的唯一 ID 用在此大型物件檔案的檔名為**msft-雲端式見證**容器。 這表示，您可以使用相同的 Microsoft Azure 儲存 Account 設定多個不同的叢集雲端見證。

當您使用雲端見證設定多個不同的相同的儲存空間 Azure 帳號叢集，單一**msft-雲端式見證**就會自動建立容器。 此容器包含每個叢集的一位大型物件檔案。

### <a name="to-create-an-azure-storage-account"></a>若要建立 Azure 儲存 account

1. 若要登入[Azure 入口網站](http://portal.azure.com)。
2. 在 [中樞] 功能表中，選取新-> 資料 + 儲存空間]-> [儲存空間 account。
3. 在 [建立儲存空間 account 頁面中，執行下列動作：
    1. 輸入儲存空間洽詢您的名稱。
    <br>儲存空間 account 名稱必須之間 3 到 24 個字元，且可能會包含數字和只有小寫字母。 必須唯一 Azure 儲存 account 名稱。
        
    2. 適用於**帳號類型**、**通用**。
    <br>您不能使用雲端見證 Blob 儲存 account。
    3. 適用於**效能**、**標準**。
    <br>您不能使用雲端見證 Azure Premium 儲存空間。
    2. 適用於**複製**、**在本機備援儲存 (LRS)**。
    <br>容錯做為朗讀資料時，需要一些一致性保證仲裁點，使用大型物件檔案。 您必須選取 therefor**儲存在本機備援**的**複寫**類型。

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>檢視及儲存便捷鍵複製 Azure 儲存洽詢您的

當您建立的 Microsoft Azure 儲存帳號時，它就關聯兩個快速鍵會自動專為主要便捷鍵和次要便捷鍵。 建立雲端見證第一次，使用**主要便捷鍵**。 有是有關使用雲端見證哪一個按鍵無限制。  

#### <a name="to-view-and-copy-storage-access-keys"></a>若要檢視和複製存放裝置便捷鍵

Azure 入口網站中瀏覽到儲存帳號，請按一下**[所有設定]**，然後按一下 [**鍵**以檢視，請複製和產生您 account 便捷鍵。 讓便捷鍵也包含預先設定的連接字串使用您的主要和次要金鑰，您可以複製（看到圖 4）將應用程式中使用。

![Microsoft Azure 管理便捷鍵對話方塊的開發進程的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)  
**儲存空間快速鍵圖 4:**

### <a name="view-and-copy-endpoint-url-links"></a>檢視及複製端點 URL 連結  
當您建立儲存空間帳號時，下列 Url 專使用的格式： `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

雲端見證一律會使用**Blob**儲存空間類型。 Azure 使用**。core.windows.net**的端點。 在雲端見證設定時，可能是，您設定的不同的端點根據您的案例（例如 Microsoft Azure 資料中心中國地區都有不同的端點）。  

> [!NOTE]  
> 端點 URL 由自動見證雲端資源和設定的任何額外的步驟會所需的 URL。  

#### <a name="to-view-and-copy-endpoint-url-links"></a>若要檢視和結束點 URL 複製連結
在 Azure 入口網站瀏覽到儲存帳號，請按一下**[所有設定]**，然後按一下 [**屬性**以檢視及複製您的端點 Url（看到圖 5）。  

![雲端見證端點連結的開發進程的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**圖 5 所示：雲端見證端點 URL 連結**

如需有關建立及管理 Azure 儲存帳號，請查看[有關 Azure 儲存帳號](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>雲端見證設定為仲裁見證叢集
雲端見證設定為在建置到容錯移轉叢集管理員現有仲裁設定精靈良好整合。  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>若要設定雲端見證仲裁見證為
1. 上市容錯移轉叢集管理員。
2. 以滑鼠右鍵按一下叢集]-> [**更多] 動作** -> **設定叢集仲裁設定**（看到圖 6）。 這時限設定叢集仲裁精靈。  
    ![功能表路徑 Configue 叢集仲裁設定容錯移轉叢集管理員 UI 中的開發進程的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png)**圖 6。叢集仲裁設定**

3. 在**選擇仲裁設定**頁面上，選取 [**選取仲裁見證**（請圖 7）。  

    ![快照 '選取 quotrum 見證' 選項叢集仲裁精靈中的按鈕](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **圖 7 所示。 選取 [仲裁設定**

4. 在**選擇仲裁見證**頁面上，選取 [**設定雲端見證**（請圖 8）。  

    ![若要選取雲端見證適當的選項按鈕的開發進程的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **圖 8。 選取 [仲裁見證**  

5. 在**設定雲端見證**頁面上，輸入下列資訊：  
    1. （必要的參數）Azure 儲存 Account 名稱。  
    2. （必要的參數）儲存空間過去對應便捷鍵。  
        1. 建立第一次時, 使用主要便捷鍵（檢視圖 5）  
        2. 旋轉主要便捷鍵時, 使用次要便捷鍵（檢視圖 5）  
    3. （選擇性參數）如果您想要使用不同的 Azure 服務端點（例如 Microsoft Azure 服務在中國），然後更新端點伺服器名稱。  

    ![雲端見證設定窗格叢集仲裁精靈中的開發進程的快照](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
    **圖 9：設定您的雲端見證**

6. 在成功雲端見證的設定，您可以檢視新建的見證資源容錯移轉叢集管理員中嵌入式管理單元（看到圖 10）。

    ![成功的雲端見證的設定](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **圖 10：的雲端見證成功的設定**

### <a name="configuring-cloud-witness-using-powershell"></a>設定使用 PowerShell 雲端見證  
現有的 Set-ClusterQuorum PowerShell 命令已新增額外的參數對應至雲端見證。  

您可以設定雲端見證使用[`Set-ClusterQuorum`](https://technet.microsoft.com/library/ee461013.aspx)下列 PowerShell 命令：  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

萬一您必須使用不同的端點（少數）：  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>使用雲端見證 azure 儲存 Account 注意事項  
在雲端見證設定為容錯移轉叢集仲裁見證時，請參考下列：
* 儲存便捷鍵，而您容錯移轉叢集會建立，及安全地儲存 [分享存取權安全性 (SAS) 預付碼。  
* 只要便捷鍵有效正確產生的 SAS 預付碼。 旋轉時主要便捷鍵，請務必第一次更新之前，請先重新存取主要次要便捷鍵雲端見證（所有您叢集上所使用的儲存空間 Account）。  
* 雲端見證使用 Azure 儲存 Account 服務的其他 HTTPS 介面。 這表示它需要開放所有叢集節點 HTTPS 連接埠。

### <a name="proxy-considerations-with-cloud-witness"></a>使用雲端見證 proxy 注意事項  
雲端見證使用 HTTPS（預設連接埠 443）建立與 Azure blob 服務通訊。 請確認透過網路 Proxy，您可以存取 HTTPS 連接埠。

## <a name="see-also"></a>也了
- [Windows Server 容錯中的新功能](whats-new-in-failover-clustering.md)
