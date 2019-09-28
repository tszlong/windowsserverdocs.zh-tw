---
title: 在虛擬機器中使用儲存空間直接存取
ms.prod: windows-server
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: 如何在虛擬機器來賓叢集中部署儲存空間直接存取-例如，在 Microsoft Azure 中。
ms.localizationpriority: medium
ms.openlocfilehash: ab0ce792c5a948e763a48493a78ccdac7a6fe74c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366054"
---
# <a name="using-storage-spaces-direct-in-guest-virtual-machine-clusters"></a>使用來賓虛擬機器叢集中的儲存空間直接存取

> 適用於：Windows Server 2019、Windows Server 2016

您可以將儲存空間直接存取部署在實體伺服器或虛擬機器來賓叢集的叢集上，如本主題中所述。 這種類型的部署會在私人或公用雲端上的一組 Vm 之間提供虛擬共用存放裝置，讓應用程式的高可用性解決方案得以用來提高應用程式的可用性。

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## <a name="deploying-in-azure-iaas-vm-guest-clusters"></a>在 Azure Iaas VM 來賓叢集中部署

[Azure 範本](https://github.com/robotechredmond/301-storage-spaces-direct-md)已發佈會降低複雜度、設定最佳做法，以及您在 AZURE Iaas VM 中儲存空間直接存取部署的速度。 這是在 Azure 中部署的建議解決方案。

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## <a name="requirements"></a>需求

在虛擬化環境中部署儲存空間直接存取時，適用下列考慮事項。

> [!TIP]
> Azure 範本會自動為您設定下列考慮，而且是在 Azure IaaS Vm 中部署時的建議解決方案。

-   最少2個節點和最多3個節點

-   2-節點部署必須設定見證（雲端見證或檔案共用見證）

-   3節點部署可容忍1個節點，並在另一個節點上遺失一或多個磁片。  如果關閉2個節點，則虛擬磁片會離線，直到其中一個節點返回為止。  

-   設定要跨容錯網域部署的虛擬機器

    -   Azure –設定可用性設定組

    -   Hyper-v –在 Vm 上設定 AntiAffinityClassNames，以將 Vm 分散到不同的節點

    -   VMware –藉由建立「個別虛擬機器」類型的 DRS 規則來設定 VM VM 反親和性規則，以將 Vm 分散到 ESX 主機。 提供給儲存空間直接存取使用的磁片應該使用 Paravirtual SCSI （PVSCSI）介面卡。 如需 Windows Server 的 PVSCSI 支援，請參閱 https://kb.vmware.com/s/article/1010398 。

-   利用低延遲/高效能儲存體-需要 Azure 進階儲存體受控磁片

-   部署未設定快取裝置的平面儲存設計

-   最少2個虛擬資料磁片呈現給每個 VM （VHD/VHDX/VMDK）

    此數目與裸機部署不同，因為虛擬磁片可以實作為不容易受到實體失敗影響的檔案。

-   藉由執行下列 PowerShell Cmdlet，在健全狀況服務中停用自動磁片磁碟機更換功能：

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   不支援：主機層級虛擬磁片快照集/還原

    相反地，請使用傳統來賓層級備份解決方案來備份和還原儲存空間直接存取磁片區上的資料。

-   若要為來賓叢集中可能的 VHD/VHDX/VMDK 儲存延遲提供更高的復原能力，請增加儲存空間 i/o 超時值：

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    十六進位7530的十進位對等是30000，也就是30秒。 請注意，預設值為1770十六進位，或 6000 Decimal，其為6秒。

## <a name="see-also"></a>另請參閱

[用於部署儲存空間直接存取、影片和逐步指南的其他 Azure IAAS VM 範本](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126)。

[其他儲存空間直接存取總覽](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
