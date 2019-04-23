---
title: 將 GUID 磁碟分割表格 (GPT) 磁碟變更為主開機記錄 (MBR) 磁碟
description: 說明如何將 GUID 磁碟分割表格 (GPT) 磁碟變更為主開機記錄 (MBR) 磁碟分割樣式磁碟。
ms.date: 06/19/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1ae755b9c41d66ce5f907f600be17547398acc1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839579"
---
# <a name="convert-a-gpt-disk-into-an-mbr-disk"></a>轉換成 MBR 磁碟的 GPT 磁碟

> **適用於：** Windows 10，Windows 8.1、 Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

主開機記錄 (MBR) 磁碟使用標準 BIOS 磁碟分割表格。 GUID 磁碟分割表格 (GPT) 磁碟則使用整合可延伸韌體介面 (UEFI)。 MBR 磁碟無法在個別磁碟上支援四個以上的磁碟分割。 不建議容量大於兩個 TB 的磁碟使用 MBR 磁碟分割方法。

只要磁碟是空的且未包含任何磁碟區，就可以將磁碟從 GPT 變更為 MBR 磁碟分割樣式。

> [!NOTE]
> 轉換磁碟之前，請備份所有資料，並關閉任何正在存取磁碟的程式。

> [!NOTE]
> 您必須至少是**備份操作員**或**系統管理員**群組的成員，才能完成這些步驟。

<a id="BKMK_WINUI"></a>

## <a name="converting-using-the-windows-interface"></a>轉換使用 Windows 介面

1.  備份或移動您要轉換成 MBR 磁碟的基本 GPT 磁碟上的所有磁碟區。

2.  如果磁碟包含任何磁碟分割或磁碟區，請個別按右鍵，然後按一下 **\[刪除磁碟區\]**。

3.  以滑鼠右鍵按一下要變更成 MBR 磁碟的 GPT 磁碟，然後按一下 **\[轉換成 MBR 磁碟\]**。

<a id="BKMK_CMD"></a>

## <a name="converting-using-a-command-line"></a>轉換使用命令列

1.  備份或移動您要轉換成 MBR 磁碟的基本 GPT 磁碟上的所有磁碟區。

2.  開啟提升權限的命令提示字元，方式為：以滑鼠右鍵按一下 **\[命令提示字元\]**，然後選擇 **\[以系統管理員身分執行\]**。

3. 輸入 `diskpart`。 如果磁碟未包含任何磁碟分割或磁碟區，請跳至步驟 6。

4.  在 **DISKPART** 提示中輸入 `list disk`。 記下您要刪除的磁碟編號。

5.  在 **DISKPART** 提示中輸入 `select disk <disknumber>`。

6.  在 **DISKPART** 提示中輸入 `clean`。

    > [!IMPORTANT]
    > 執行 **clean** 命令將會刪除磁碟上的所有磁碟分割或磁碟區。

7.  在 **DISKPART** 提示中輸入 `convert mbr`。

<br />

| 值 | 描述 |
| --- | --- |
| <p>**清單中的磁碟**</p> | <p>顯示磁碟的清單和這些磁碟的相關資訊，例如大小、可用空間數量、磁碟是基本還是動態磁碟，以及磁碟使用的是主開機記錄 (MBR) 還是 GUID 磁碟分割表格 (GPT) 磁碟分割樣式。 標示有星號 (*) 的磁碟具有焦點。</p> |
| <p>**選取磁碟**</p> | <p>選取指定的磁碟 (其中 <em>disknumber</em> 是磁碟編號)，並讓它成為焦點。</p> | <p>**clean**</p> | <p>從焦點所在的磁碟移除所有的磁碟分割或磁碟區。</p> |
| <p>**轉換 mbr**</p> | <p>將使用 GUID 磁碟分割表格 (GPT) 磁碟分割樣式的空白基本磁碟轉換為使用主開機記錄 (MBR) 磁碟分割樣式的基本磁碟。</p>

## <a name="see-also"></a>另請參閱

-   [命令列語法標記法](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


