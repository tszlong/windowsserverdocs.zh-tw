---
title: 將動態磁碟變更回基本磁碟
description: 說明如何將動態磁碟變更回基本磁碟。
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 249db6d2779e696ef93fecfd11718dbcce8654be
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812463"
---
# <a name="change-a-dynamic-disk-back-to-a-basic-disk"></a>將動態磁碟變更回基本磁碟

> **適用於：** Windows 10，Windows 8.1、 Windows Server （半年通道）、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主題說明如何刪除動態磁碟上的所有資料，然後將它轉換回基本磁碟。 動態磁碟在 Windows 中已被取代，我們不建議再使用它們。 相反地，我們建議使用基本磁碟，或使用較新[儲存空間](https://support.microsoft.com/help/12438/windows-10-storage-spaces)技術，當您想要集區磁碟一起成較大的磁碟區。 如果您想要鏡像處理 Windows 用來開機的磁碟區，您可能需要使用硬體 RAID 控制器，像許多主機板上都會包含的這種控制器。

> [!WARNING]
> 若要將動態磁碟轉換回到基本磁碟，您必須從磁碟刪除所有磁碟區，並永久清除磁碟上的所有資料。 請務必先備份您想要保留的任何資料，再繼續進行作業。

## <a name="changing-a-dynamic-disk-back-to-a-basic-disk"></a>將動態磁碟變更回基本磁碟

-   [使用 Windows 介面](#to-change-a-dynamic-disk-back-to-a-basic-disk-using-the-windows-interface)
-   [使用命令列](#to-change-a-dynamic-disk-back-to-a-basic-disk-using-a-command-line)

> [!NOTE]
> 您必須至少是**備份操作員**或**系統管理員**群組的成員，才能完成這些步驟。

#### <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-using-the-windows-interface"></a>若要使用 Windows 介面將動態磁碟變更回基本磁碟

1.  備份要從動態轉換為基本之磁碟上的所有磁碟區。

2.  在 [磁碟管理] 中，以滑鼠右鍵按一下每個要轉換為基本磁碟的動態磁碟，然後針對磁碟上的每個磁碟區按一下 **\[刪除磁碟區\]** 。

3.  刪除磁碟上的所有磁碟區後，按右鍵，然後按一下 **\[轉換成基本磁碟\]** 。

#### <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-using-a-command-line"></a>若要使用命令列將動態磁碟變更回基本磁碟

1.  備份要從動態轉換為基本之磁碟上的所有磁碟區。

2.  開啟命令提示字元，然後輸入 `diskpart`：

3.  在 **DISKPART** 提示中輸入 `list disk`。 記下要轉換為基本磁碟的磁碟編號。

4.  在 **DISKPART** 提示中輸入 `select disk <disknumber>`。

5.  在 **DISKPART** 提示中輸入 `detail disk <disknumber>`。

6.  在 **DISKPART** 提示中，針對磁碟上的每個磁碟區輸入 `select volume= <volumenumber>`，然後輸入 `delete volume`。

7.  在 **DISKPART** 提示中，輸入 `select disk <disknumber>`，並指定要轉換為基本磁碟的磁碟編號。

8.  在 **DISKPART** 提示中輸入 `convert basic`。


| 值  | 描述 |
| --- | --- |
| **清單中的磁碟**                         | 顯示磁碟的清單和這些磁碟的相關資訊，例如大小、可用空間數量、磁碟是基本還是動態磁碟，以及磁碟使用的是主開機記錄 (MBR) 還是 GUID 磁碟分割表格 (GPT) 磁碟分割樣式。 標示有星號 (*) 的磁碟具有焦點。 |
| **選取磁碟** <em>disknumber</em>   | 選取指定的磁碟 (其中 <em>disknumber</em> 是磁碟編號)，並讓它成為焦點。  |
| **詳細資料磁碟** <em>disknumber</em>   | 顯示所選磁碟的內容以及該磁碟上的磁碟區。  |
| **選取磁碟區** <em>disknumber</em> | 選取指定的磁碟區 (其中 <em>disknumber</em> 是磁碟區編號)，並讓它成為焦點。 如果沒有指定磁碟區，**select** 會命令列出焦點所在的目前磁碟區。 您可以用編號、磁碟機代號或掛接點路徑來指定磁碟區。 在基本磁碟上，選取磁碟區也會讓對應的磁碟分割成為焦點。 |
| **刪除磁碟區**                     | 刪除選取的磁碟區。 您無法刪除系統磁碟區、開機磁碟區或任何包含使用中分頁檔或損毀傾印 (記憶體傾印) 的磁碟區。 |
| **基本轉換** | 將空的動態磁碟轉換成基本磁碟。  |

## <a name="additional-considerations"></a>其他考量

-   磁碟必須沒有包含任何磁碟區或資料，才能變更回基本磁碟。 如果想要保留您的資料，請先進行備份或將資料移到其他磁碟，再將磁碟轉換成基本磁碟。
-   將動態磁碟變更回基本磁碟後，您在該磁碟上只能建立磁碟分割和邏輯磁碟機。

## <a name="see-also"></a>另請參閱

-   [命令列語法標記法](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)