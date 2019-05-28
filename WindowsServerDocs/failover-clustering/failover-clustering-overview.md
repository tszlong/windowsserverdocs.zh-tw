---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: 容錯移轉叢集
ms.prod: windows-server-threshold
layout: LandingPage
ms.topic: landing-page
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 03/08/2019
ms.localizationpriority: high
ms.openlocfilehash: 9e4184e52ef48e758ebc80e63d3d6f952a09cc2c
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222449"
---
# <a name="failover-clustering-in-windows-server"></a>Windows Server 中的容錯移轉叢集

> 適用於：Windows Server 2019，Windows Server 2016

>[!TIP]
> 尋找舊版 Windows Server 的相關資訊嗎？ 查看我們其他位於 docs.microsoft.com 的 [Windows Server 文件庫](/previous-versions/windows/)。 您也可以[搜尋這個網站](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)以取得特定資訊。

<hr />

容錯移轉叢集是由獨立電腦組成的群組，它們共同運作以提升叢集角色 (之前稱為叢集應用程式和服務) 的可用性及擴充性。 各個叢集伺服器 (稱為節點) 是透過實體纜線與軟體連接。 如果其中一或多個叢集節點失敗，其他節點即會透過稱為「容錯移轉」的程序開始提供服務。 不僅如此，叢集角色還會主動監看以確認服務正常運作。 如果服務沒有運作，就會重新啟動或移到另一個節點。

容錯移轉叢集也提供叢集共用磁碟區 (CSV) 功能，此功能提供一致的分散式命名空間，叢集角色可使用這個空間從所有節點存取共用存放空間。 透過容錯移轉叢集功能，使用者幾乎不會感覺到服務中斷的情況。

容錯叢集有許多實用的應用程式，包括：
* Microsoft SQL Server 和 Hyper-V 虛擬機器之類應用程式的高可用性或持續可用的檔案共用儲存區
* 在實體伺服器或安裝在執行 Hyper-V 伺服器的虛擬機器上執行的高可用性叢集角色


|  |  |
|---------|---------|
|![新功能](../media/i-whats-new.svg)  | [**什麼是容錯移轉叢集的新功能**](whats-new-in-failover-clustering.md) |


|  |  |  |
|---------|---------|---------|
|![了解](../media/i-cluster.svg)**了解**  |  ![規劃](../media/i-cluster.svg)**規劃**  |  ![部署](../media/i-cluster.svg)**部署**       |
| [針對應用程式資料向外延展檔案伺服器](sofs-overview.md)    |   [規劃容錯移轉叢集硬體需求及存放選項](clustering-requirements.md)      |  [Active Directory 網域服務中部署預先設置叢集電腦物件](prestage-cluster-adds.md)  |
|  [叢集和集區仲裁](../storage/storage-spaces/understand-quorum.md)   |   [使用叢集共用磁碟區 (CSV)](failover-cluster-csvs.md)      | [建立容錯移轉叢集](create-failover-cluster.md)        |
|  [容錯網域感知](fault-domains.md)   |  [搭配儲存空間直接存取使用客體虛擬機器叢集](../storage/storage-spaces/storage-spaces-direct-in-vm.md)       | [部署雙節點檔案伺服器](../storage/storage-spaces/storage-spaces-direct-in-vm.md)        |
| [簡化的 SMB 多重通道和多 NIC 叢集網路](smb-multichannel.md)    |         |  [管理仲裁及見證](manage-cluster-quorum.md)       |
|   [VM 負載平衡](vm-load-balancing-overview.md)  |         |   [部署雲端見證](deploy-cloud-witness.md)      |
|   [叢集集合](../storage/storage-spaces/cluster-sets.md)  |         |     [部署檔案共用見證](file-share-witness.md)    |
|   [叢集同質](cluster-affinity.md)  |         |    [叢集作業系統輪流升級]()     |
|     |         |     [在相同的硬體上升級容錯移轉叢集](upgrade-option-same-hardware.md)    |
|     |         |     [部署 Active Directory 中斷連結的叢集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\))    |


|  |  |  |
|---------|---------|---------|
|![管理](../media/i-cluster.svg)**管理**  |  ![工具和設定](../media/i-cluster.svg)**工具和設定**  |  ![社群資源](../media/i-cluster.svg)**社群資源**       |
| [叢集感知更新](cluster-aware-updating.md)    |   [容錯移轉叢集的 PowerShell Cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)      |  [高可用性 (叢集) 論壇](https://go.microsoft.com/fwlink/p/?LinkId=230641)       |
|  [健康情況服務](health-service-overview.md)   |   [叢集感知更新的 PowerShell Cmdlet](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps)      | [容錯移轉叢集和網路負載平衡團隊部落格](http://blogs.msdn.com/b/clustering/)        |
|  [叢集網域移轉](cluster-domain-migration.md)   |         |         |
|  [使用 Windows 錯誤報告進行疑難排解](troubleshooting-using-wer-reports.md)   |         |         |
