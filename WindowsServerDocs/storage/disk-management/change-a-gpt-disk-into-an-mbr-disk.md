---
title: 將 GUID 磁碟分割表格 (GPT) 磁碟變更為主開機記錄 (MBR) 磁碟
description: 描述如何將 GUID 磁碟分割表格 (GPT) 磁碟變更為主開機記錄 (MBR) 磁碟分割樣式磁碟。
ms.date: 06/19/2018
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4fc402dbf46944930da8ee803b1440149bc09309
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87935897"
---
# <a name="convert-a-gpt-disk-into-an-mbr-disk"></a>將 GPT 磁碟轉換為 MBR 磁碟

> **適用於：** Windows 10、Windows 8.1、Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

主開機記錄 (MBR) 磁碟使用標準 BIOS 磁碟分割表格。 GUID 磁碟分割表格 (GPT) 磁碟則使用整合可延伸韌體介面 (UEFI)。 MBR 磁碟無法在個別磁碟上支援四個以上的磁碟分割。 不建議容量大於兩個 TB 的磁碟使用 MBR 磁碟分割方法。

只要磁碟是空的且未包含任何磁碟區，就可以將磁碟從 GPT 變更為 MBR 磁碟分割樣式。

> [!NOTE]
> 轉換磁碟之前，請先備份磁碟上的任何資料，並關閉任何正在存取磁碟的程式。

> [!NOTE]
> 您必須至少是**備份操作員**或**系統管理員**群組的成員，才能完成這些步驟。

## <a name="converting-using-the-windows-interface"></a>使用 Windows 介面進行轉換

1.  在您要轉換成 MBR 磁碟的基本 GPT 磁碟上備份或移動所有磁碟區。

2.  如果磁碟包含任何磁碟分割或磁碟區，請以滑鼠右鍵按一下每個磁碟分割或磁碟區，然後按一下 [刪除磁碟區]  。

3.  以滑鼠右鍵按一下要變更成 MBR 磁碟的 GPT 磁碟，然後按一下 [轉換成 MBR 磁碟]  。

## <a name="converting-using-a-command-line"></a>使用命令列進行轉換

1.  在您要轉換成 MBR 磁碟的基本 GPT 磁碟上備份或移動所有磁碟區。

2.  以滑鼠右鍵按一下 [命令提示字元]  ，然後選擇 [以系統管理員身分執行]  ，以開啟提升權限的命令提示字元。

3. 輸入 `diskpart`。 如果磁碟未包含任何磁碟分割或磁碟區，請跳至步驟 6。

4.  在 **DISKPART** 提示中鍵入 `list disk`。 記下您要刪除的磁碟編號。

5.  在 **DISKPART** 提示中鍵入 `select disk <disknumber>`。

6.  在 **DISKPART** 提示中鍵入 `clean`。

    > [!IMPORTANT]
    > 執行 **clean** 命令將會刪除磁碟上的所有磁碟分割或磁碟區。

7.  在 **DISKPART** 提示中鍵入 `convert mbr`。

|                值                  |      說明   |
| ------------------------------------- | -----------------  |
|  <strong>list disk</strong>  | 顯示磁碟的清單和這些磁碟的相關資訊，例如大小、可用空間數量、磁碟是基本還是動態磁碟，以及磁碟使用的是主開機記錄 (MBR) 還是 GUID 磁碟分割表格 (GPT) 磁碟分割樣式。 標示有星號 (\*) 的磁碟具有焦點。 |
| <strong>select disk</strong> |                                                                                                          選取指定的磁碟 (其中 <em>disknumber</em> 是磁碟編號)，並讓它成為焦點。                                                                                                           |
| <strong>convert mbr</strong> |                                                                               將使用 GUID 磁碟分割表格 (GPT) 磁碟分割樣式的空白基本磁碟轉換為使用主開機記錄 (MBR) 磁碟分割樣式的基本磁碟。                                                                                |

## <a name="see-also"></a>另請參閱

-   [Command-line syntax notation](/previous-versions/orphan-topics/ws.11/cc742449(v=ws.11)) (命令列語法標記法)
