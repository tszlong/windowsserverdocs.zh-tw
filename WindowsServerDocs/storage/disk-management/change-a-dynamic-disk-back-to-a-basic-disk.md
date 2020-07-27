---
title: 將動態磁碟變更回基本磁碟
description: 描述如何將動態磁碟轉換回基本磁碟。
ms.date: 12/18/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f866127bc43df307ac1a6a1fa44daac91ada5dcd
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966080"
---
# <a name="change-a-dynamic-disk-back-to-a-basic-disk"></a>將動態磁碟變更回基本磁碟

> **適用於：** Windows 10、Windows 8.1、Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題描述如何刪除動態磁碟上的所有資料，然後將它轉換回基本磁碟。 動態磁碟在 Windows 中已淘汰，不建議繼續使用。 相反地，當您想要將磁碟一起彙集成更大的磁碟區時，建議使用基本磁碟或使用新的[儲存空間](https://support.microsoft.com/help/12438/windows-10-storage-spaces)技術。 如果您想要鏡像處理 Windows 用來開機的磁碟區，您可能需要使用硬體 RAID 控制器，例如許多主機板上都會包含的這種控制器。

> [!WARNING]
> 若要將動態磁碟轉換回基本磁碟，您必須從磁碟刪除所有磁碟區，並永久清除磁碟上的所有資料。 請務必先備份您想要保留的任何資料，再繼續進行作業。

## <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-by-using-disk-management"></a>使用磁碟管理將動態磁碟變更回基本磁碟

1.  在您要從動態轉換為基本的磁碟上備份所有磁碟區。

2. 使用系統管理員權限開啟 [磁碟管理]。

   執行此動作的簡易方法，是在工作列上的 [搜尋] 方塊中輸入**電腦管理**，選取並按住 (以滑鼠右鍵按一下) [電腦管理]，然後選取 [以系統管理員身分執行] > [是]。 在開啟 [電腦管理] 後，移至 [存放裝置] > [磁碟管理]。

2.  在 [磁碟管理] 中，在要轉換為基本磁碟的動態磁碟上選取並按住 (或以滑鼠右鍵按一下) 每個磁碟區，然後按一下 [刪除磁碟區]。

3.  刪除磁碟上的所有磁碟區後，以滑鼠右鍵按一下磁碟，然後按一下 [轉換成基本磁碟]。

## <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-by-using-a-command-line"></a>使用命令列將動態磁碟變更回基本磁碟

1.  在您要從動態轉換為基本的磁碟上備份所有磁碟區。

2.  開啟命令提示字元，然後輸入 `diskpart`：

3.  在 **DISKPART** 提示中鍵入 `list disk`。 記下您要轉換為基本的磁碟編號。

4.  在 **DISKPART** 提示中鍵入 `select disk <disknumber>`。

5.  在 **DISKPART** 提示中鍵入 `detail disk`。

6.  在 **DISKPART** 提示中，針對磁碟上的每個磁碟區鍵入 `select volume= <volumenumber>`，然後鍵入 `delete volume`。

7.  在 **DISKPART** 提示中鍵入 `select disk <disknumber>`，並指定要轉換為基本磁碟之磁碟的磁碟編號。

8.  在 **DISKPART** 提示中鍵入 `convert basic`。

| 值  | 說明 |
| --- | --- |
| **list disk**                         | 顯示磁碟清單和這些磁碟的相關資訊，例如大小、可用空間數量、磁碟是基本還是動態磁碟，以及磁碟使用的是主開機記錄 (MBR) 還是 GUID 磁碟分割表格 (GPT) 磁碟分割樣式。 標示有星號 (*) 的磁碟具有焦點。 |
| **select disk** <em>disknumber</em>   | 選取指定的磁碟 (其中 <em>disknumber</em> 是磁碟編號)，並讓它成為焦點。  |
| **detail disk** <em>disknumber</em>   | 顯示所選磁碟的內容以及該磁碟上磁碟區。  |
| **select volume** <em>disknumber</em> | 選取指定的磁碟區 (其中 <em>disknumber</em> 是磁碟區編號)，並讓它成為焦點。 如果沒有指定磁碟區，**select** 命令會列出焦點所在的目前磁碟區。 您可以用編號、磁碟機代號或掛接點路徑來指定磁碟區。 在基本磁碟上，選取磁碟區也會讓對應的磁碟分割成為焦點。 |
| **delete volume**                     | 刪除選取的磁碟區。 您無法刪除系統磁碟區、開機磁碟區，或是任何包含使用中分頁檔或損毀傾印 (記憶體傾印) 的磁碟區。 |
| **convert basic** | 將空的動態磁碟轉換成基本磁碟。  |

## <a name="additional-considerations"></a>其他考量

-   磁碟必須沒有包含任何磁碟區或資料，才能變更回基本磁碟。 如果想要保留您的資料，請先進行備份或將資料移到其他磁碟區，再將磁碟轉換成基本磁碟。
-   將動態磁碟變更回基本磁碟後，您在該磁碟上只能建立磁碟分割和邏輯磁碟機。

## <a name="additional-references"></a>其他參考資料

-   [Command-line syntax notation](/previous-versions/orphan-topics/ws.11/cc742449(v=ws.11)) (命令列語法標記法)
