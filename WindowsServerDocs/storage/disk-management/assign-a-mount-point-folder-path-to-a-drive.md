---
title: 將掛接點資料夾路徑指派給磁碟機。
description: 本文描述如何將掛接點資料夾路徑 (而非磁碟機代號) 指派給磁碟機。
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b2fda216b57fbf036ce20c40b4c8b38d44404f3c
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80815531"
---
# <a name="assign-a-mount-point-folder-path-to-a-drive"></a>將掛接點資料夾路徑指派給磁碟機

> **適用於：** Windows 10、Windows 8.1、Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用 [磁碟管理] 將掛接點資料夾路徑 (而非磁碟機代號) 指派給磁碟機。 掛接點資料夾路徑只能在基本或動態 NTFS 磁碟區的空資料夾上使用。

## <a name="assigning-a-mount-point-folder-path-to-a-drive"></a>將掛接點資料夾路徑指派給磁碟機

> [!NOTE]
> 您必須至少是**備份操作員**或**系統管理員**群組的成員，才能完成這些步驟。

#### <a name="to-assign-a-mount-point-folder-path-to-a-drive-by-using-the-windows-interface"></a>使用 Windows 介面將掛接點資料夾路徑指派給磁碟機

1.  在 [磁碟管理] 中，以滑鼠右鍵按一下您要指派掛接點資料夾路徑的磁碟分割或磁碟區。 
2. 按一下 [變更磁碟機代號及路徑]  ，然後按一下 [新增]  。 
3. 按一下 [掛在下列空的 NTFS 資料夾上]  。
4. 鍵入 NTFS 磁碟區上的空資料夾路徑，或按一下 [瀏覽]  來尋找資料夾。

#### <a name="to-assign-a-mount-point-folder-path-to-a-drive-using-a-command-line"></a>使用命令列將掛接點資料夾路徑指派給磁碟機

1.  開啟命令提示字元，然後輸入 `diskpart`：

2.  在 **DISKPART** 提示中鍵入 `list volume`，並記下您要指派路徑的磁碟區編號。

3.  在 **DISKPART** 提示中鍵入 `select volume <volumenumber>`。 

4. 選取您要指派路徑的簡單磁碟區 *volumenumber*。

5.  在 **DISKPART** 提示中鍵入 `assign [mount=<path>]`。

#### <a name="to-remove-a-mount-point-folder-path-to-a-drive"></a>移除磁碟機的掛接點資料夾路徑

-   若要移除掛接點資料夾路徑，請按一下路徑，然後按一下 [移除]  。

| 值 | 說明 |
| --- | --- |
| **list volume** | 顯示所有磁碟上的基本和動態磁碟區清單。 |
| **select volume**        | 選取指定的磁碟區 (其中 <em>volumenumber</em> 是磁碟區編號)，並讓它成為焦點。 如果沒有指定磁碟區，**select** 命令會列出焦點所在的目前磁碟區。 您可以用編號、磁碟機代號或掛接點資料夾路徑來指定磁碟區。 在基本磁碟上，選取磁碟區也會讓對應的磁碟分割成為焦點。|
| **assign** | <ul><li> 將磁碟機代號或掛接點資料夾路徑指派給焦點所在的磁碟區。 如果沒有指定磁碟機代號或掛接點資料夾路徑，則指派下一個可用的磁碟機代號。 如果磁碟機代號或掛接點資料夾路徑已在使用中，就會產生錯誤。</li>  <li>您可以使用 **assign** 命令，變更與抽取式磁碟機建立關聯的磁碟機代號。</li> <li> 您無法將磁碟機代號指派給開機磁碟區或包含分頁檔的磁碟區。 此外，您也無法將磁碟機代號指派給原始設備製造商 (OEM) 磁碟分割、EFI 系統磁碟分割，或基本資料磁碟分割以外的任何 GPT 磁碟分割。</li></ul> |
| **mount=** <em>path</em> | 指定掛接磁碟位置所在空的現有 NTFS 資料夾。  |

## <a name="additional-considerations"></a>其他考量

-   如果您正在管理本機或遠端電腦，您可以瀏覽該電腦上的 NTFS 資料夾。
-   掛接點資料夾路徑只能在基本或動態 NTFS 磁碟區的空資料夾上使用。
-   若要修改掛接點資料夾路徑，請先將該路徑移除，再使用新的位置建立新資料夾路徑。 您無法直接修改掛接點資料夾路徑。
-   指派掛接點資料夾路徑給磁碟機時，請使用 [事件檢視器]  檢查系統記錄檔是否有任何指出掛接點資料夾路徑失敗的叢集服務錯誤或警告。 這些錯誤會在 [來源]  欄中列為 [ClusSvc]  ，並在 [類別]  欄中列為 [實體磁碟資源]  。
-   您也可以使用 [mountvol](https://go.microsoft.com/fwlink/?linkid=64111) 命令建立掛接磁碟。

## <a name="see-also"></a>另請參閱
-   [Command-line syntax notation](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx) (命令列語法標記法)


