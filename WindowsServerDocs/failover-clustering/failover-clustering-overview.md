---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: 容錯移轉叢集
ms.topic: landing-page
manager: lizross
author: JasonGerend
ms.author: jgerend
ms.date: 06/06/2019
ms.localizationpriority: high
ms.openlocfilehash: 41f5eef75e20a4da740141620493d2daa254b0a1
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992861"
---
# <a name="failover-clustering-in-windows-server"></a>Windows Server 中的容錯移轉叢集

> 適用於：Windows Server 2019、Windows Server 2016

容錯移轉叢集是由獨立電腦組成的群組，它們共同運作以提升叢集角色 (之前稱為叢集應用程式和服務) 的可用性及擴充性。 各個叢集伺服器 (稱為節點) 是透過實體纜線與軟體連接。 如果其中一或多個叢集節點失敗，其他節點即會透過稱為「容錯移轉」的程序開始提供服務。 不僅如此，叢集角色還會主動監看以確認服務正常運作。 如果服務沒有運作，就會重新啟動或移到另一個節點。

容錯移轉叢集也提供叢集共用磁碟區 (CSV) 功能，此功能提供一致的分散式命名空間，叢集角色可使用這個空間從所有節點存取共用存放空間。 透過容錯移轉叢集功能，使用者幾乎不會感覺到服務中斷的情況。

容錯叢集有許多實用的應用程式，包括：

* Microsoft SQL Server 和 Hyper-V 虛擬機器之類應用程式的高可用性或持續可用的檔案共用儲存區
* 在實體伺服器或安裝在執行 Hyper-V 伺服器的虛擬機器上執行的高可用性叢集角色

| **了解**                                                               |  **規劃**                          |  **部署**       |
| -------------                                                                |  --------------                        | --------------------- |
| [容錯移轉叢集中的新功能](whats-new-in-failover-clustering.md)    | [規劃容錯移轉叢集硬體需求及存放選項](clustering-requirements.md)  | [建立容錯移轉叢集](create-failover-cluster.md) |
| [針對應用程式資料向外延展檔案伺服器](sofs-overview.md)               | [使用叢集共用磁碟區 (CSV)](failover-cluster-csvs.md) | [部署雙節點檔案伺服器](deploy-two-node-clustered-file-server.md) |
|  [叢集和集區仲裁](../storage/storage-spaces/understand-quorum.md)   |  [使用客體虛擬機器叢集搭配儲存空間直接存取](../storage/storage-spaces/storage-spaces-direct-in-vm.md)       | [在 Active Directory Domain Services 中預先設置叢集電腦物件](prestage-cluster-adds.md) |
| [容錯網域感知](fault-domains.md)                                 |                                 | [在 Active Directory 中設定叢集帳戶](configure-ad-accounts.md) |
| [簡化的 SMB 多重通道和多 NIC 叢集網路](smb-multichannel.md) |                       | [管理仲裁和見證](manage-cluster-quorum.md) |
| [VM 負載平衡](vm-load-balancing-overview.md)                         |                             | [部署雲端見證](deploy-cloud-witness.md) |
| [叢集集合](../storage/storage-spaces/cluster-sets.md)                  |                             |[部署檔案共用見證](file-share-witness.md) |
| [叢集同質](cluster-affinity.md)                                     |                            | [叢集作業系統輪流升級](cluster-operating-system-rolling-upgrade.md) |
|                                                                             |                            | [在相同的硬體上升級容錯移轉叢集](upgrade-option-same-hardware.md) |
|                                                                            |                             | [部署已中斷連結 Active Directory 的叢集](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\))

|**管理**  |  **工具及設定**  |  **社群資源**       |
| ------------- |  -------------- | --------------------- |
| [叢集感知更新](cluster-aware-updating.md)    |   [容錯移轉叢集 PowerShell Cmdlet](/powershell/module/failoverclusters/?view=win10-ps)      |  [高可用性 (叢集) 論壇](https://go.microsoft.com/fwlink/p/?LinkId=230641)       |
|  [健康情況服務](health-service-overview.md)   |   [叢集感知更新 PowerShell Cmdlet](/powershell/module/clusterawareupdating/?view=win10-ps)      | [容錯移轉叢集和網路負載平衡小組部落格](https://blogs.msdn.com/b/clustering/)        |
|  [叢集網域移轉](cluster-domain-migration.md)   |         |         |
|  [使用 Windows 錯誤報告進行疑難排解](troubleshooting-using-wer-reports.md)   |         |         |