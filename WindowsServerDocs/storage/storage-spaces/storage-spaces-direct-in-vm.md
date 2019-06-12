---
title: 使用 虛擬機器中的 儲存空間直接存取
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: 如何在虛擬機器客體叢集中-例如，在 Microsoft Azure 中部署儲存空間直接存取。
ms.localizationpriority: medium
ms.openlocfilehash: b99e750b78654df48ad3b412269511d047e3057c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447817"
---
# <a name="using-storage-spaces-direct-in-guest-virtual-machine-clusters"></a>在客體虛擬機器叢集中使用儲存空間直接存取

> 適用於：Windows Server 2019，Windows Server 2016

在叢集上的實體伺服器，或在虛擬機器客體叢集，如本主題所述，您可以部署儲存空間直接存取。 這種部署類型會跨一組 Vm 的私人或公用雲端上提供虛擬共用儲存體，讓應用程式高可用性解決方案可用來增加應用程式的可用性。

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## <a name="deploying-in-azure-iaas-vm-guest-clusters"></a>在 Azure Iaas VM 客體叢集部署

[Azure 範本](https://github.com/robotechredmond/301-storage-spaces-direct-md)已發行的減少複雜程度，請設定最佳作法，以及您在 Azure Iaas VM 中的儲存空間直接存取部署速度。 這是在 Azure 中部署的建議的解決方案。

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## <a name="requirements"></a>需求

虛擬化環境中部署儲存空間直接存取時，就會適用下列考量。

> [!TIP]
> Azure 範本會自動設定以下也是建議的解決方案，在 Azure IaaS Vm 中部署時的考量。

-   2 個節點的最小和最多 3 個節點

-   2 個節點部署必須設定見證 （雲端見證或檔案共用見證）

-   3 個節點部署可以容忍 1 個節點關閉和 1 個以上的磁碟，另一個節點上的損失。  如果 2 個節點都關閉然後虛擬磁碟我們會離線直到其中一個節點傳回。  

-   設定要部署範圍跨容錯網域的虛擬機器

    -   Azure-設定可用性設定組

    -   Hyper-V – 將跨節點的 Vm 在 Vm 上設定 AntiAffinityClassNames

    -   VMware-設定 VM 反親和性規則，藉由建立 DRS 的規則類型 ' 不同的虛擬機器 」 來分隔在 ESX 主機的 Vm。 可供使用儲存空間直接存取使用的磁碟應使用 Paravirtual SCSI (PVSCSI) 配接器。 如需 Windows Server PVSCSI 支援，請參閱 https://kb.vmware.com/s/article/1010398。

-   利用低延遲 / 高效能儲存體-Azure 進階儲存體受控磁碟所需

-   部署裝置尚未快取設定的一般儲存體設計

-   最少 2 個呈現給每個 VM 的虛擬資料磁碟 (VHD / VHDX / VMDK)

    這個數字是不同於裸機部署的因為虛擬磁碟可以實作為不容易遭受實體失敗的檔案。

-   執行下列 PowerShell cmdlet 來停用自動的磁碟機取代功能，在 健全狀況服務：

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   不支援：主機層級的虛擬磁碟的快照集/還原

    改為使用傳統的客體層級的備份解決方案來備份和還原的儲存空間直接存取磁碟區上的資料。

-   若要更高的恢復功能給可能的 VHD / VHDX / VMDK 客體叢集中的儲存體延遲增加儲存體空間 I/O 逾時值：

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    十進位對等的十六進位 7530 是 30000，也就是 30 秒。 請注意，預設值為 1770年十六進位或 6000 Decimal，也就是 6 秒。

## <a name="see-also"></a>另請參閱

[其他的 Azure Iaas VM 範本部署儲存空間直接存取、 影片和逐步指南](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/)。

[額外的儲存空間直接存取概觀](https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
