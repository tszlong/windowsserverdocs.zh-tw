---
title: 使用 Windows 管理中心管理容錯移轉叢集
description: 使用 Windows 管理中心管理容錯移轉叢集（Project 檀香山）
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: b7f015ac4c9906447069501bf0922b36306a51d7
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950489"
---
# <a name="manage-failover-clusters-with-windows-admin-center"></a>使用 Windows 管理中心管理容錯移轉叢集

>適用於：Windows Admin Center、Windows Admin Center 預覽版

> [!Tip]
> 對 Windows Admin Center 不熟悉？
> [下載或深入瞭解 Windows 系統管理中心](../overview.md)。

## <a name="managing-failover-clusters"></a>管理容錯移轉叢集
[容錯移轉](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview)叢集是一種 Windows Server 功能，可讓您將多部伺服器組成一個容錯叢集，以提高應用程式和服務的可用性和擴充性，例如向外延展檔案伺服器、hyper-v 和 Microsoft SQL Server。

雖然您可以在 Windows 系統管理中心將容錯移轉叢集節點新增為[伺服器](manage-servers.md)連線來將它們當做個別伺服器來管理，但您也可以將它們新增為容錯移轉叢集，以查看和管理叢集資源、存放裝置、網路、節點、角色、虛擬機器和虛擬交換器。

![容錯移轉叢集總覽畫面](../media/manage-failover-clusters/fcm-overview.png)

## <a name="adding-a-failover-cluster-to-windows-admin-center"></a>將容錯移轉叢集新增至 Windows 管理中心
若要將叢集新增至 Windows 系統管理中心：

1. 按一下 [所有連接] 底下的 [ **+ 新增**]。
2. 選擇新增**容錯移轉連接**。
3. 輸入叢集的名稱，如果出現提示，則為要使用的認證。
4. 在 Windows 系統管理中心，您可以選擇將叢集節點新增為個別的伺服器連接。
5. 按一下 [**提交**] 完成。

叢集將會新增至 [總覽] 頁面上的連接清單。 按一下以連接到叢集。

> [!NOTE]
> 您也可以在 Windows 管理中心新增叢集作為[超](manage-hyper-converged.md)交集叢集連線，以管理超融合叢集。

## <a name="tools"></a>工具

下列工具適用于容錯移轉叢集連接：

| 工具 | 說明 |
| ---- | ----------- |
| 概觀 | 查看容錯移轉叢集詳細資料和管理叢集資源 |
| [磁碟] | 查看叢集共用磁片和磁片區 |
| [網路] | 在叢集中查看網路 |
| 節點 | 查看和管理叢集節點 |
| 角色 | 管理叢集角色或建立空白角色 |
| 更新 | 管理叢集感知更新（需要[CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)） |
| [虛擬機器](manage-virtual-machines.md) | 查看和管理虛擬機器 |
| 虛擬交換器 | 查看和管理虛擬交換器 |

## <a name="more-coming"></a>更多即將推出

Windows 管理中心內的容錯移轉叢集管理正積極開發，未來將會新增新功能。 您可以在 UserVoice 中查看功能的狀態和投票：

|功能要求|
|-------|
| [顯示更多叢集磁片資訊](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [支援其他叢集動作](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [支援在不同叢集上執行 Hyper-v 和向外延展檔案伺服器的聚合式叢集](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [查看 CSV 區塊快取](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [查看全部或提議新功能](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |