---
title: 將主開機記錄 (MBR) 磁碟變更成 GUID 磁碟分割表格 (GPT) 磁碟
description: 說明如何將主開機記錄 (MBR) 變更成 GUID 磁碟分割表格 (GPT) 磁碟
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 902a845bbe6a7e2a4d811aac0ea2990fb3557832
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812459"
---
# <a name="convert-an-mbr-disk-into-a-gpt-disk"></a>將 MBR 磁碟轉換成 GPT 磁碟

> **適用於：** Windows 10，Windows 8.1、 Windows Server （半年通道）、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

主開機記錄 (MBR) 磁碟使用標準 BIOS 磁碟分割表格。 GUID 磁碟分割表格 (GPT) 磁碟則使用整合可延伸韌體介面 (UEFI)。 GPT 磁碟的其中一個優點是，您在每個磁碟上可以有四個以上的磁碟分割。 容量大於兩個 TB 磁碟也需要 GPT。

只要磁碟沒有包含任何磁碟分割或磁碟區，就可以將磁碟從 MBR 變更為 GPT 磁碟分割樣式。

> [!NOTE]
> 轉換磁碟之前，請備份所有資料，並關閉任何正在存取磁碟的程式。

> [!NOTE]
> 您必須至少是**備份操作員**或**系統管理員**群組的成員，才能完成這些步驟。

## <a name="converting-using-the-windows-interface"></a>轉換使用 Windows 介面

1.  備份或移動您要轉換成 GPT 磁碟的基本 MBR 磁碟上的資料。

2.  如果磁碟包含任何磁碟分割或磁碟區，請個別按右鍵，然後按一下 **\[刪除磁碟分割\]** 或 **\[刪除磁碟區\]** 。

3.  以滑鼠右鍵按一下要變更成 GPT 磁碟的 MBR 磁碟，然後按一下 **\[轉換成 GPT 磁碟\]** 。

## <a name="converting-using-a-command-line"></a>轉換使用命令列

您可以使用下列步驟來將空的 MBR 磁碟轉換成 GPT 磁碟。 另外還有 MBR2GPT。EXE 的工具，您可以使用，但這樣有點複雜，-請參閱[轉換的 MBR 磁碟分割為 GPT](https://docs.microsoft.com/windows/deployment/mbr-to-gpt)如需詳細資訊。

1.  備份或移動您要轉換成 GPT 磁碟的基本 MBR 磁碟上的資料。

2.  開啟提升權限的命令提示字元，方式為：以滑鼠右鍵按一下 **\[命令提示字元\]** ，然後選擇 **\[以系統管理員身分執行\]** 。

3. 輸入 `diskpart`。 如果磁碟未包含任何磁碟分割或磁碟區，請跳至步驟 6。

4.  在 **DISKPART** 提示中輸入 `list disk`。 記下要轉換的磁碟編號。

5.  在 **DISKPART** 提示中輸入 `select disk <disknumber>`。

6.  在 **DISKPART** 提示中輸入 `clean`。

    > [!NOTE]
    > 執行 **clean** 命令將會刪除磁碟上的所有磁碟分割或磁碟區。

7.  在 **DISKPART** 提示中輸入 `convert gpt`。

| 值  | 描述  |
| ----- | ---- |
| **清單中的磁碟** | 顯示磁碟的清單和這些磁碟的相關資訊，例如大小、可用空間數量、磁碟是基本還是動態磁碟，以及磁碟使用的是主開機記錄 (MBR) 還是 GUID 磁碟分割表格 (GPT) 磁碟分割樣式。 標示有星號 (*) 的磁碟具有焦點。 |
| **選取磁碟** *disknumber* | 選取指定的磁碟 (其中 *disknumber* 是磁碟編號)，並讓它成為焦點。 |
| **clean** | 從焦點所在的磁碟移除所有的磁碟分割或磁碟區。  |
| **轉換 gpt**| 將使用主開機記錄 (MBR) 磁碟分割樣式的空白基本磁碟轉換成使用 GUID 磁碟分割表格 (GPT) 磁碟分割樣式的基本磁碟。 |

## <a name="see-also"></a>另請參閱

-   [命令列語法標記法](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)