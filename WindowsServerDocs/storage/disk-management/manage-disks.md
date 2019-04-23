---
title: 管理磁碟
description: 本文說明如何管理磁碟
ms.date: 12/21/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f4698dac683ff3769eb4403ae2750ad38a301022
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846189"
---
# <a name="manage-disks"></a>管理磁碟

> **適用於：** Windows 10，Windows 8.1、 Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主題及子主題將討論如何使用磁碟管理來管理在電腦中，磁碟，並包含初始化新的磁碟，將磁碟不同的磁碟分割樣式，以及 Windows 如何處理新磁碟的線上狀態之間轉換的相關資訊。

## <a name="online-and-offline-status"></a>線上及離線狀態

磁碟管理 」 會顯示是否磁碟在線上 （可），或離線。

在 Windows 中，所有新探索到的磁碟預設都會上線，並且具有讀取與寫入權限。 在 Windows Server 中，所有新探索到的磁碟預設都會上線，並且具有讀取與寫入權限，但要是這些磁碟是在共用匯流排 (例如 SCSI、iSCSI、序列連結 SCSI 或光纖通道) 則除外。 在共用匯流排上的磁碟是第一次偵測到離線。

如果磁碟離線，必須先上線，您才能進行初始化或在其中建立磁碟區。

若要讓磁碟連線，或使其離線，以滑鼠右鍵按一下磁碟名稱，然後選擇 適當的動作。





## <a name="see-also"></a>另請參閱

-   [初始化新的磁碟](initialize-new-disks.md)
-   [將磁碟移至另一部電腦](move-disks-to-another-computer.md)
-   [將動態磁碟變更回基本磁碟](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [將主開機記錄磁碟變更為 GUID 磁碟分割表格磁碟](change-an-mbr-disk-into-a-gpt-disk.md)
-   [將 GUID 磁碟分割表格磁碟變更為主開機記錄磁碟](change-a-gpt-disk-into-an-mbr-disk.md)
-   [管理虛擬硬碟](manage-virtual-hard-disks.md)