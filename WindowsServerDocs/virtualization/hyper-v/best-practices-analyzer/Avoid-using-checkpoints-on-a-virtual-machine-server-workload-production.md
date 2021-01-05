---
title: 避免在生產環境中執行伺服器工作負載的虛擬機器上使用檢查點
description: 瞭解當找到具有一或多個檢查點的虛擬機器時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 1be75890-d316-495a-b9b7-be75fc1aac10
ms.date: 8/16/2016
ms.openlocfilehash: 0fef612d9da5ad74561429c3852d407623d9d334
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834563"
---
# <a name="avoid-using-checkpoints-on-a-virtual-machine-that-runs-a-server-workload-in-a-production-environment"></a>避免在生產環境中執行伺服器工作負載的虛擬機器上使用檢查點

>適用於：Windows Server 2016



*如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|作業|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

> [!NOTE]
> 在 Windows Server 2012 R2 中，虛擬機器快照集已重新命名為 Hyper-v 管理員中的虛擬機器檢查點，以符合 System Center 虛擬機器管理中使用的術語。 如需詳細資訊，請參閱 [檢查點和快照集總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn818483(v=ws.11))。

## <a name="issue"></a>問題

*找到有一或多個檢查點的虛擬機器。*

## <a name="impact"></a>影響

*可用空間可能會在儲存檢查點檔案的實體磁片上執行。如果發生這種情況，就不能在實體儲存體上執行任何額外的磁片作業。依賴實體儲存體的虛擬機器可能會受到影響。*

如果實體磁碟空間已用盡，則任何在該磁片上儲存檢查點或虛擬硬碟的執行中虛擬機器，都可能會自動暫停。 [Hyper-v 管理員] 會將這些虛擬機器的狀態顯示為 [已暫停]。

## <a name="resolution"></a>解決方案

*如果虛擬機器在生產環境中執行伺服器工作負載，請讓虛擬機器離線，然後使用 Hyper-v 管理員來套用或刪除檢查點。若要刪除檢查點，您必須關閉虛擬機器才能完成此程式。*

> [!NOTE]
> 生產檢查點現在可作為標準檢查點的替代方案。 如需詳細資訊，請參閱 [在標準或生產檢查點之間選擇](../manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)。