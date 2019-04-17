---
title: 使用虛擬機器中的儲存空間直接存取
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: 如何在虛擬機器客體叢集-例如，在 Microsoft Azure 中部署儲存空間直接存取。
ms.localizationpriority: medium
ms.openlocfilehash: a741475d09ab7f7ab29f158ef55378ca9a37beac
ms.sourcegitcommit: 52883945fd6e6bb5c24bd81944ca4bc0c5e6a216
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "8964610"
---
# 在客體虛擬機器的叢集中使用儲存空間直接存取

> 適用於： Windows Server 2019、 Windows Server 2016

您可以部署儲存空間直接存取叢集上的實體伺服器，或在虛擬機器客體叢集，如本主題中所討論。 這種類型的部署會跨一組 Vm 的上方私人或公用雲端提供虛擬共用存放裝置，讓應用程式的高可用性解決方案可以用來增加應用程式的可用性。

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## 在 Azure Iaas VM 客體叢集部署

[Azure 範本](https://github.com/robotechredmond/301-storage-spaces-direct-md)已發行的降低複雜性、 設定最佳做法，與您 Azure Iaas VM 的儲存空間直接存取部署的速度。 這是在 Azure 中部署的建議的解決方案。

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## 需求

下列考量適用於虛擬化環境中部署儲存空間直接存取時。

> [!TIP]
> Azure 範本將會自動設定下列考量適用於您且 Azure IaaS Vm 中部署時的建議解決方案。

-   2 個節點的最小和最大的 3 個節點

-   2-節點部署必須設定見證 （雲端見證或檔案共用見證）

-   部署 3 節點可容許 1 向下的節點，然後 1 或以上的磁碟，另一個節點上的損失。  如果 2 個節點都已關機再虛擬磁碟我們會離線直到其中一個節點傳回。  

-   設定部署到容錯網域部虛擬機器

    -   Azure – 設定可用性集

    -   Hyper-v-V – 來分隔 Vm 跨節點 Vm 上設定 AntiAffinityClassNames

    -   VMware – 設定 VM VM 反親和性規則，藉由建立 DRS 規則的類型 ' 個別的虛擬機器 」 來分隔跨 ESX 主機的虛擬機器。 呈現以搭配儲存空間直接存取磁碟應該使用 Paravirtual SCSI (PVSCSI) 介面卡。 使用 Windows Server PVSCSI 支援，請參閱https://kb.vmware.com/s/article/1010398。

-   利用低延遲 / 高效能存放裝置-Azure Premium 存放裝置管理磁碟是必要項目

-   使用設定任何快取裝置部署單層式儲存體設計

-   提供給每個 VM 的 2 個虛擬資料磁碟的最小值 (VHD / VHDX / VMDK)

    這個數字是不同於裸機部署，因為虛擬磁碟可以實作為不容易受到實體失敗的檔案。

-   停用自動磁碟機更換功能健全狀況服務中的執行下列 PowerShell cmdlet:

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   不支援： 裝載層級的虛擬磁碟快照/還原

    改為使用傳統客體層級的備份解決方案來備份及還原儲存空間直接存取磁碟區上的資料。

-   若要提供更大的復原到可能的 VHD / VHDX / VMDK 儲存延遲客體叢集中的增加儲存空間 I/O 逾時值：

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    十六進位 7530 的小數點對等項目是 30000，也就是 30 秒。 請注意的預設值是 1770年十六進位或 6000 小數點，也就是 6 秒。

## 請參閱

[適用於部署儲存空間直接存取、 影片及逐步指南額外 Azure Iaas VM 範本](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/)。

[額外的儲存空間直接存取概觀](https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
