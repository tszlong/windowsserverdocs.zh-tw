---
title: "管理磁碟"
description: "本文說明如何管理磁碟"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ae96071733b961fbe65551120894c21c633db83e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="manage-disks"></a>管理磁碟

> **適用於：**Windows 10、Windows 8.1、Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題及其子主題討論使用 [磁碟管理] 管理電腦中的磁碟，並提供有關在不同磁碟分割樣式之間轉換磁碟，以及 Windows 處理新磁碟連線狀態方式的資訊。

## <a name="converting-disk-types"></a>轉換磁碟類型

雖然 [磁碟管理] 可讓您在不同類型及磁碟分割樣式之間變更磁碟，但有些作業是無法復原的動作 (除非將磁碟機格式化)。 您應該仔細考慮使用最適合您的應用程式的磁碟類型和磁碟分割樣式。

下表顯示在各種不同磁碟分割樣式之間轉換磁碟的選項：

| 磁碟類型 | 轉換成 MBR  | 轉換成 GPT| 轉換成動態磁碟 |
| ---- | --- | --- |--- |
| <p>主開機記錄 (MBR)</p> | <p>--</p> | <p>允許 (如果磁碟上沒有磁碟區)。</p> | <p>允許，但磁碟可能變成無法開機。 請參閱附註。</p> |
| <p>GUID 磁碟分割表格 (GPT)</p> | <p>允許 (如果磁碟上沒有磁碟區)。</p> | <p>--</p>  | <p>允許，但磁碟可能變成無法開機。 請參閱附註。</p> |


> [!NOTE]
> 在多重開機案例中，如果您已開機進入一個作業系統，並將包含其他作業系統的基本 MBR 磁碟轉換為動態磁碟時，就再也無法開機進入這個其他作業系統。

## <a name="online-and-offline-status"></a>連線與離線狀態

[磁碟管理] 會顯示磁碟的連線與離線狀態。 

在 Windows 中，所有新探索到的磁碟預設都會上線，並且具有讀取與寫入權限。 在 Windows Server 中，所有新探索到的磁碟預設都會上線，並且具有讀取與寫入權限，但要是這些磁碟是在共用匯流排 (例如 SCSI、iSCSI、序列連結 SCSI 或光纖通道) 則除外。 共用匯流排上的磁碟會在第一次偵測到時離線。

如果磁碟離線，必須先上線，您才能進行初始化或在其中建立磁碟區。 

-  以滑鼠右鍵按一下磁碟名稱，然後選擇適當動作，即可使磁碟上線或離線。

## <a name="see-also"></a>另請參閱

-   [初始化新磁碟](initialize-new-disks.md)
-   [將磁碟移到另一部電腦](move-disks-to-another-computer.md)
-   [將動態磁碟變更回基本磁碟](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [將主開機記錄磁碟變更成 GUID 磁碟分割表格磁碟](change-an-mbr-disk-into-a-gpt-disk.md)
-   [將 GUID 磁碟分割表格磁碟變更成主開機記錄磁碟](change-a-gpt-disk-into-an-mbr-disk.md)
-   [管理虛擬硬碟](manage-virtual-hard-disks.md)