---
title: 在虛擬機器中使用儲存空間直接存取
description: 如何在虛擬機器來賓叢集中部署儲存空間直接存取-例如，在 Microsoft Azure 中。
ms.prod: windows-server
ms.author: eldenc
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
ms.localizationpriority: medium
ms.openlocfilehash: 77f82023b8ed5db6f329530bebc3162cb8565856
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474545"
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

- 最少2個節點和最多3個節點

- 2-節點部署必須設定見證（雲端見證或檔案共用見證）

- 3節點部署可容忍1個節點，並在另一個節點上遺失一或多個磁片。  如果關閉2個節點，則虛擬磁片會離線，直到其中一個節點返回為止。

- 設定要跨容錯網域部署的虛擬機器

    - Azure –設定可用性設定組

    - Hyper-v –在 Vm 上設定 AntiAffinityClassNames，以將 Vm 分散到不同的節點

    - VMware –藉由建立「個別虛擬機器」類型的 DRS 規則來設定 VM VM 反親和性規則，以將 Vm 分散到 ESX 主機。 提供給儲存空間直接存取使用的磁片應該使用 Paravirtual SCSI （PVSCSI）介面卡。 如需 Windows Server 的 PVSCSI 支援，請參閱 https://kb.vmware.com/s/article/1010398 。

- 利用低延遲/高效能儲存體-需要 Azure 進階儲存體受控磁片

- 部署未設定快取裝置的平面儲存設計

- 最少2個虛擬資料磁片呈現給每個 VM （VHD/VHDX/VMDK）

    此數目與裸機部署不同，因為虛擬磁片可以實作為不容易受到實體失敗影響的檔案。

- 藉由執行下列 PowerShell Cmdlet，在健全狀況服務中停用自動磁片磁碟機更換功能：

    ```powershell
          Get-storagesubsystem clus* | set-storagehealthsetting -name "System.Storage.PhysicalDisk.AutoReplace.Enabled" -value "False"
          ```

- To give greater resiliency to possible VHD / VHDX / VMDK storage latency in guest clusters, increase the Storage Spaces I/O timeout value:

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    The decimal equivalent of Hexadecimal 7530 is 30000, which is 30 seconds. Note that the default value is 1770 Hexadecimal, or 6000 Decimal, which is 6 seconds.

## Not supported

- Host level virtual disk snapshot/restore

    Instead use traditional guest level backup solutions to backup and restore the data on the Storage Spaces Direct volumes.

- Host level virtual disk size change

    The virtual disks exposed through the virtual machine must retain the same size and characteristics. Adding more capacity to the storage pool can be accomplished by adding more virtual disks to each of the virtual machines and adding them to the pool. It's highly recommended to use virtual disks of the same size and characteristics as the current virtual disks.

## Additional References

- [Additional Azure Iaas VM templates for deploying Storage Spaces Direct, videos, and step-by-step guides](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126).

- [Additional Storage Spaces Direct Overview](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
