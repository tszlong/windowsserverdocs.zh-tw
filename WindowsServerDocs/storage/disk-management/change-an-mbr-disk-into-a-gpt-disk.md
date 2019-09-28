---
title: 將主開機記錄 (MBR) 磁碟變更成 GUID 磁碟分割表格 (GPT) 磁碟
description: 描述如何將主開機記錄 (MBR) 磁碟變更成 GUID 磁碟分割表格 (GPT) 磁碟
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6bd97802fbef342520e92a857a1a53acf3e8d7a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385944"
---
# <a name="convert-an-mbr-disk-into-a-gpt-disk"></a>將 MBR 磁碟轉換為 GPT 磁碟

> **適用於：** Windows 10、Windows 8.1、Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

主開機記錄 (MBR) 磁碟使用標準 BIOS 磁碟分割表格。 GUID 磁碟分割表格 (GPT) 磁碟則使用整合可延伸韌體介面 (UEFI)。 GPT 磁碟的其中一個優點是，您在每個磁碟上可以有四個以上的磁碟分割。 容量大於兩個 TB 的磁碟也需要 GPT。

只要磁碟沒有包含任何磁碟分割或磁碟區，就可以將磁碟從 MBR 變更為 GPT 磁碟分割樣式。

> [!NOTE]
> 轉換磁碟之前，請先備份磁碟上的任何資料，並關閉任何正在存取磁碟的程式。

> [!NOTE]
> 您必須至少是**備份操作員**或**系統管理員**群組的成員，才能完成這些步驟。

## <a name="converting-using-the-windows-interface"></a>使用 Windows 介面進行轉換

1.  備份或移動您要轉換成 GPT 磁碟的基本 MBR 磁碟上資料。

2.  如果磁碟包含任何磁碟分割或磁碟區，請以滑鼠右鍵按一下每個磁碟分割或磁碟區，然後按一下 [刪除磁碟分割]  或 [刪除磁碟區]  。

3.  以滑鼠右鍵按一下要變更成 GPT 磁碟的 MBR 磁碟，然後按一下 [轉換成 GPT 磁碟]  。

## <a name="converting-using-a-command-line"></a>使用命令列進行轉換

使用下列步驟將空的 MBR 磁碟轉換成 GPT 磁碟。 您也可以使用 MBR2GPT.EXE 工具，但有點複雜；如需詳細資訊，請參閱 [Convert MBR partition to GPT](https://docs.microsoft.com/windows/deployment/mbr-to-gpt) (將 MBR 磁碟分割轉換為 GPT)。

1.  備份或移動您要轉換成 GPT 磁碟的基本 MBR 磁碟上資料。

2.  以滑鼠右鍵按一下 [命令提示字元]  ，然後選擇 [以系統管理員身分執行]  ，以開啟提升權限的命令提示字元。

3. 輸入 `diskpart`。 如果磁碟未包含任何磁碟分割或磁碟區，請跳至步驟 6。

4.  在 **DISKPART** 提示中鍵入 `list disk`。 記下您要轉換的磁碟編號。

5.  在 **DISKPART** 提示中鍵入 `select disk <disknumber>`。

6.  在 **DISKPART** 提示中鍵入 `clean`。

    > [!NOTE]
    > 執行 **clean** 命令將會刪除磁碟上的所有磁碟分割或磁碟區。

7.  在 **DISKPART** 提示中鍵入 `convert gpt`。

| 值  | 描述  |
| ----- | ---- |
| **list disk** | 顯示磁碟清單和這些磁碟的相關資訊，例如大小、可用空間數量、磁碟是基本還是動態磁碟，以及磁碟使用的是主開機記錄 (MBR) 還是 GUID 磁碟分割表格 (GPT) 磁碟分割樣式。 標示有星號 (*) 的磁碟具有焦點。 |
| **select disk** *disknumber* | 選取指定的磁碟 (其中 *disknumber* 是磁碟編號)，並讓它成為焦點。 |
| **clean** | 從焦點所在的磁碟移除所有磁碟分割或磁碟區。  |
| **convert gpt**| 將使用主開機記錄 (MBR) 磁碟分割樣式之空白基本磁碟轉換成使用 GUID 磁碟分割表格 (GPT) 磁碟分割樣式的基本磁碟。 |

## <a name="see-also"></a>另請參閱

-   [Command-line syntax notation](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx) (命令列語法標記法)