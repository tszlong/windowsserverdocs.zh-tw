---
title: 管理容錯移轉叢集，使用 Windows Admin Center
description: 管理容錯移轉叢集，使用 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 63f5fca8e7ef63200e01cfc6e00a979e7f045b51
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296670"
---
# 管理容錯移轉叢集，使用 Windows Admin Center

>適用於：Windows Admin Center、Windows Admin Center 預覽版

> [!Tip]
> 對 Windows Admin Center 不熟悉？
> [深入了解 Windows Admin Center](../understand/windows-admin-center.md) 或[立即下載](https://aka.ms/windowsadmincenter).

## 管理容錯移轉叢集
[容錯移轉叢集](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview)是一項 Windows Server 功能，可讓您將多部伺服器分組到容錯叢集來提高可用性和延展性的應用程式和服務，例如向外延展檔案伺服器、 HYPER-V 和Microsoft SQL Server。

雖然您可以透過將人員新增作為 Windows Admin Center 中的[伺服器連線](manage-servers.md)，做為個別伺服器管理容錯移轉叢集節點，您也可以新增它們為容錯移轉叢集，若要檢視和管理叢集資源、 儲存空間、 網路、 節點、 角色、 虛擬電腦和虛擬交換器。

![容錯移轉叢集概觀畫面](../media/manage-failover-clusters/fcm-overview.png)

## 加入 Windows Admin Center 中的容錯移轉叢集
若要新增對 Windows Admin Center 的叢集：

1. 按一下 [ **+ 新增**下所有的連線。
2. 選擇加入**容錯移轉連線**。
3. 輸入的叢集名稱，如果出現提示，使用的認證。
4. 您必須將叢集節點新增為 Windows Admin Center 中的個別伺服器連線的選項。
5. 按一下 [**送出**以完成。

叢集將會新增到您在 [概觀] 頁面上的連線清單。 按一下以連線到叢集。

> [!NOTE]
> 您也可以管理超融合式叢集藉由新增叢集 Windows Admin Center 中的[超融合式叢集連線](manage-hyper-converged.md)。

## 工具

下列工具可供容錯移轉叢集連線：

| 工具 | 描述 |
| ---- | ----------- |
| 概觀 | 檢視容錯移轉叢集的詳細資料和管理叢集資源 |
| 磁碟 | 檢視叢集共用磁碟和磁碟區 |
| 網路 | 在叢集中的檢視網路 |
| 節點 | 檢視和管理叢集節點 |
| 角色 | 管理叢集角色，或建立空白的角色 |
| 更新 | 管理叢集感知更新 （需要[CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)） |
| [虛擬機器](manage-virtual-machines.md) | 檢視和管理虛擬機器 |
| 虛擬交換器 | 檢視和管理虛擬交換器 |

## 未來的更多

Windows Admin Center 中的容錯移轉叢集管理會主動開發和未來會新增的新功能。 您可以檢視 UserVoice 狀態和投票的功能：

|功能要求|
|-------|
| [顯示更多的叢集的磁碟資訊](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [支援其他叢集動作](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [支援融合式的叢集不同的叢集上執行 HYPER-V 與向外延展檔案伺服器](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [檢視 CSV 區塊快取](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [查看所有或建議的新功能](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |