---
title: 在虛擬機器中使用儲存空間直接存取
description: 如何在虛擬機器來賓叢集中部署儲存空間直接存取，例如 Microsoft Azure。
ms.author: eldenc
manager: eldenc
ms.topic: article
author: eldenchristensen
ms.date: 07/15/2020
ms.localizationpriority: medium
ms.openlocfilehash: c21570d964db48e4336031315a13657f3777cc72
ms.sourcegitcommit: 2365a7b23e2eccd13be350306c622d2ad9d36bc8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2020
ms.locfileid: "96788067"
---
# <a name="using-storage-spaces-direct-in-guest-virtual-machine-clusters"></a>在來賓虛擬機器叢集中使用儲存空間直接存取

> 適用於：Windows Server 2019、Windows Server 2016

您可以在實體伺服器叢集或虛擬機器來賓叢集（如本主題所述）的叢集上部署儲存空間直接存取 (（有時稱為 S2D) ）。 這種類型的部署會在私人或公用雲端上的一組 Vm 上提供虛擬共用儲存體，讓應用程式高可用性解決方案可以用來提高應用程式的可用性。

若要改為使用 Azure 共用磁片作為來賓虛擬機器，請參閱 [Azure 共用磁片](/azure/virtual-machines/windows/disks-shared)。

![儲存空間直接存取圖表](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## <a name="deploying-in-azure-iaas-vm-guest-clusters"></a>在 Azure Iaas VM 來賓叢集中部署

[Azure 範本](https://github.com/robotechredmond/301-storage-spaces-direct-md) 已發佈降低複雜性、設定最佳作法，以及您在 AZURE Iaas VM 中儲存空間直接存取部署的速度。 這是在 Azure 中部署的建議解決方案。

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## <a name="requirements-for-guest-clusters"></a>來賓叢集的需求

在虛擬化環境中部署儲存空間直接存取時，適用下列考慮事項。

> [!TIP]
> Azure 範本會為您自動設定下列考慮，而且是在 Azure IaaS Vm 中部署時的建議解決方案。

- 最少2個節點和最多3個節點

- 2-節點部署必須設定見證 (雲端見證或檔案共用見證) 

- 3個節點的部署可以容忍1個節點，並在另一個節點上遺失1或多個磁片。  如果有2個節點關閉，則虛擬磁片會離線，直到其中一個節點傳回為止。

- 設定要跨容錯網域部署的虛擬機器

    - Azure-設定可用性設定組

    - Hyper-v –在 Vm 上設定 AntiAffinityClassNames，以將 Vm 跨節點分隔

    - VMware –藉由建立類型為「個別虛擬機器」的 DRS 規則，將 vm 與 ESX 主機分開，來設定 VM-VM 反親和性規則。 呈現給儲存空間直接存取使用的磁片應該使用 Paravirtual SCSI (PVSCSI) 介面卡。 如需 Windows Server 的 PVSCSI 支援，請參閱 https://kb.vmware.com/s/article/1010398 。

- 利用低延遲/高效能儲存體-需要 Azure 進階儲存體受控磁片

- 部署未設定快取裝置的平面儲存體設計

- 每個 VM (VHD/VHDX/VMDK) 最少顯示2個虛擬資料磁片

    此數目與裸機部署不同，因為虛擬磁片可以實作為不容易受到實體故障影響的檔案。

- 藉由執行下列 PowerShell Cmdlet，在健全狀況服務中停用自動磁片磁碟機更換功能：

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name "System.Storage.PhysicalDisk.AutoReplace.Enabled" -value "False"
    ```

- 若要為來賓叢集中可能的 VHD/VHDX/VMDK 儲存體延遲提供更高的復原能力，請增加儲存空間 i/o 超時值：

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    十六進位7530的十進位對等專案是30000，也就是30秒。 請注意，預設值是1770十六進位或 6000 Decimal，也就是6秒。

## <a name="not-supported"></a>不支援

- 主機層級虛擬磁片快照集/還原

    相反地，您可以使用傳統的來賓層級備份解決方案來備份和還原儲存空間直接存取磁片區上的資料。

- 主機層級虛擬磁片大小變更

    透過虛擬機器公開的虛擬磁片必須保留相同的大小和特性。 將更多的虛擬磁片新增至每部虛擬機器，並將它們新增至集區，即可完成將更多容量新增至存放集區。 強烈建議使用與目前虛擬磁片相同大小和特性的虛擬磁片。

## <a name="additional-references"></a>其他參考資料

- [用於部署儲存空間直接存取、影片和逐步指南的其他 Azure IAAS VM 範本](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126)。

- [其他儲存空間直接存取總覽](./storage-spaces-direct-overview.md)
