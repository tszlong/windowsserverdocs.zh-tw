---
title: 管理 Windows Admin Center 使用的容錯移轉叢集
description: 管理 Windows Admin Center （專案檀香山） 與容錯移轉叢集
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f7e14581f7f6b14b0cf39308de236b68a07e8c9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824069"
---
# <a name="manage-failover-clusters-with-windows-admin-center"></a>管理 Windows Admin Center 使用的容錯移轉叢集

>適用於：Windows Admin Center，Windows Admin Center 預覽

> [!Tip]
> 對 Windows Admin Center 不熟悉？
> [深入了解 Windows Admin Center](../understand/windows-admin-center.md) 或[立即下載](https://aka.ms/windowsadmincenter).

## <a name="managing-failover-clusters"></a>管理容錯移轉叢集
[容錯移轉叢集](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview)是 Windows Server 功能，可讓您將多部伺服器一起容錯叢集，以增加可用性和延展性的應用程式和服務，例如向外延展檔案伺服器、 HYPER-V 和Microsoft SQL Server。

雖然您也可以將它們做為個別的伺服器管理容錯移轉叢集節點[伺服器連線](manage-servers.md)在 Windows Admin Center，您也可以將它們為容錯移轉叢集來檢視和管理叢集資源、 儲存體、 網路、 節點角色、 虛擬機器和虛擬交換器。

![容錯移轉叢集概觀畫面](../media/manage-failover-clusters/fcm-overview.png)

## <a name="adding-a-failover-cluster-to-windows-admin-center"></a>將容錯移轉叢集新增至 Windows Admin Center
將叢集新增至 Windows Admin Center:

1. 按一下  **+ 新增**下所有連接。
2. 選擇新增**容錯移轉連線**。
3. 輸入叢集的名稱，如果出現提示，要使用的認證。
4. 您必須將叢集節點新增為 Windows Admin Center 中的個別伺服器連接選項。
5. 按一下 **送出**才能完成。

連線清單在 [概觀] 頁面上，將會新增叢集。 按一下以連線到叢集。

> [!NOTE]
> 您也可以管理超交集叢集新增為叢集[Hyper-Converged 叢集連線](manage-hyper-converged.md)Windows Admin Center 中。

## <a name="tools"></a>工具

下列工具可供容錯移轉叢集的連線：

| 工具 | 描述 |
| ---- | ----------- |
| 總覽 | 檢視容錯移轉叢集的詳細資料和管理叢集資源 |
| [磁碟] | 檢視叢集共用磁碟和磁碟區 |
| [網路] | 檢視叢集中的網路 |
| 節點 | 檢視及管理叢集節點 |
| 角色 | 管理叢集角色，或建立空白的角色 |
| 更新 | 管理叢集感知更新 |
| [虛擬機器](manage-virtual-machines.md) | 檢視及管理虛擬機器 |
| 虛擬交換器 | 檢視及管理虛擬交換器 |

## <a name="more-coming"></a>更多即將推出

容錯移轉叢集管理的 Windows Admin Center 正在開發，並將在不久的將來加入新功能。 您可以檢視 狀態 和 票選功能在 UserVoice 中：

|功能要求|
|-------|
| [顯示更多的叢集的磁碟資訊](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [支援其他的叢集動作](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [支援聚合式的叢集在不同的叢集上執行 HYPER-V 與向外延展檔案伺服器](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [檢視 CSV 區塊快取](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [查看所有或提出新功能](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |