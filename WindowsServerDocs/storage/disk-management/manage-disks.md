---
title: 管理磁碟
description: 本文描述如何管理磁碟
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0aac0b78e79949de94ebd20912b8c2b2db167339
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "71385867"
---
# <a name="manage-disks"></a>管理磁碟

> **適用於：** Windows 10、Windows 8.1、Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題及其副主題討論如何使用 [磁碟管理] 管理電腦中的磁碟，並提供有關將新磁碟初始化、在不同磁碟分割樣式之間轉換磁碟，以及 Windows 處理新磁碟線上狀態方式的資訊。

## <a name="online-and-offline-status"></a>線上及離線狀態

[磁碟管理] 會顯示磁碟為線上 (可用) 或離線。

在 Windows 中，所有新探索到的磁碟預設都會上線，且具有讀取與寫入權限。 在 Windows Server 中，所有新探索到的磁碟預設都會上線，且具有讀取與寫入權限，但若這些磁碟是在共用匯流排 (例如 SCSI、iSCSI、序列連結 SCSI 或光纖通道) 則除外。 共用匯流排上的磁碟會在第一次偵測到時離線。

如果磁碟離線，則您必須先上線才能加以初始化或在其中建立磁碟區。

若要使磁碟上線或離線，請以滑鼠右鍵按一下磁碟名稱，然後選擇適當動作。

## <a name="see-also"></a>另請參閱

-   [初始化新磁碟](initialize-new-disks.md)
-   [將磁碟移至另一部電腦](move-disks-to-another-computer.md)
-   [將動態磁碟變更回基本磁碟](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [將主開機記錄磁碟變更成 GUID 磁碟分割表格磁碟](change-an-mbr-disk-into-a-gpt-disk.md)
-   [將 GUID 磁碟分割表格磁碟變更成主開機記錄磁碟](change-a-gpt-disk-into-an-mbr-disk.md)
-   [管理虛擬硬碟](manage-virtual-hard-disks.md)