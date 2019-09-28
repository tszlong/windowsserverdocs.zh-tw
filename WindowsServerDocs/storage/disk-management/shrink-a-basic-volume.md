---
title: 壓縮基本磁碟區
description: 本文描述如何壓縮基本磁碟區
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2baf24ed656ef06d44dff93180701d25e6852500
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385856"
---
# <a name="shrink-a-basic-volume"></a>壓縮基本磁碟區

> **適用於：** Windows 10、Windows 8.1、Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以透過將主要磁碟分割及邏輯磁碟機壓縮至同一磁碟上相鄰的連續空間，以減少它們的使用空間。 例如：如果您發現您需要一個額外的磁碟分割，卻沒有額外的磁碟，您可以從磁碟區的結尾處壓縮現有磁碟分割，以建立供新磁碟分割使用的新未配置空間。 壓縮作業可能會因為存在特定檔案類型而遭封鎖。 如需詳細資訊，請參閱[其他考量](#additional-considerations) 

當您壓縮磁碟分割時，將會自動重新配置磁碟上任何一般檔案以建立新的未配置空間。 不需要將磁碟重新格式化來壓縮磁碟分割。

> [!CAUTION]
> 如果磁碟分割是包含資料 (例如資料庫檔案) 的原始磁碟分割 (也就是沒有檔案系統的磁碟分割)，壓縮磁碟分割可能會損毀資料。

## <a name="shrinking-a-basic-volume"></a>壓縮基本磁碟區

> [!NOTE]
> 您必須至少是**備份操作員**或**系統管理員**群組的成員，才能完成這些步驟。

#### <a name="to-shrink-a-basic-volume-using-the-windows-interface"></a>使用 Windows 介面壓縮基本磁碟區

1.  在 [磁碟管理員] 中，以滑鼠右鍵按一下您想要壓縮的基本磁碟區。

2.  按一下 [壓縮磁碟區]  。

3.  遵循畫面上的指示操作。


> [!NOTE]
> 您只能壓縮沒有檔案系統或使用 NTFS 檔案系統的基本磁碟區。

#### <a name="to-shrink-a-basic-volume-using-a-command-line"></a>使用命令列壓縮基本磁碟區

1.  開啟命令提示字元，然後輸入 `diskpart`：

2.  在 **DISKPART** 提示中鍵入 `list volume`。 記下您要壓縮的簡單磁碟區編號。

3.  在 **DISKPART** 提示中鍵入 `select volume <volumenumber>`。 選取您要壓縮的簡單磁碟區 *volumenumber*。

4.  在 **DISKPART** 提示中鍵入 `shrink [desired=<desiredsize>] [minimum=<minimumsize>]`。 可能的話，將選取的磁碟區壓縮到 *desiredsize* (以 MB 為單位)，如果 *desiredsize* 太大，則壓縮到 *minimumsize*。

| 值             | 描述 |
| ---               | ----------- |
| **list volume** | 顯示所有磁碟上的基本和動態磁碟區清單。 |
| **select volume** | 選取指定的磁碟區 (其中 <em>volumenumber</em> 是磁碟區編號)，並讓它成為焦點。 如果沒有指定磁碟區，**select** 命令會列出焦點所在的目前磁碟區。 您可以用編號、磁碟機代號或掛接點路徑來指定磁碟區。 在基本磁碟上，選取磁碟區也會讓對應的磁碟分割成為焦點。 |
| **shrink** | 壓縮具有焦點的磁碟區以建立未經配置空間。 不會有任何資料遺失。 如果磁碟分割包含無法移動的檔案 (例如分頁檔或陰影複製存放區域)，磁碟區將會壓縮到無法移動的檔案所在位置為止。 |
| **desired=** <em>desiredsize</em> | 要復原到目前磁碟分割的空間數量，以 MB 為單位。 |
| **minimum=** <em>minimumsize</em> | 至少要復原到目前磁碟分割的空間數量，以 MB 為單位。 如果未指定想要的大小或最小大小，命令將會回收盡可能最大的空間數量。 |

## <a name="additional-considerations"></a>其他考量

-   當您壓縮磁碟分割時，無法自動重新配置特定檔案 (例如分頁檔或陰影複製存放區域)，且無法將配置的空間減少到超出無法移動的檔案所在位置以外。 如果壓縮作業失敗，請檢查應用程式記錄檔的事件 259，這可識別無法移動的檔案。 如果您知道哪些叢集與導致壓縮操作未能進行的檔案建立關聯，也可以在命令提示字元使用 **fsutil** 命令 (鍵入 **fsutil volume querycluster /?** 以了解使用方式)。 當您提供 **querycluster** 參數時，命令輸出將會找出導致壓縮操作未能順利進行的無法移動檔案。
在某些情況下，您可以暫時重新配置檔案。 例如，如果您需要進一步壓縮磁碟分割，可以使用 [控制台]，將分頁檔或儲存的陰影複製移到其他磁碟、刪除儲存的陰影複製、壓縮磁碟區，然後將分頁檔移回原來的磁碟。 如果動態錯誤叢集重新對應所偵測到的錯誤叢集數目太高，您就無法壓縮磁碟分割。 如果發生這種情況，您應該考慮移動資料並更換磁碟。

-  請勿使用區塊層級複製來傳送資料。 這會將損毀的磁區也一起複製，新磁碟便會將即使是正常的相同磁區視為已損毀。

-   您可以壓縮原始磁碟分割 (沒有檔案系統的磁碟分割) 上的主要磁碟分割和邏輯磁碟機，或是使用 NTFS 檔案系統的磁碟分割。

## <a name="see-also"></a>另請參閱

-   [管理基本磁碟區](manage-basic-volumes.md)