---
title: "將掛接點資料夾路徑指派給磁碟機。"
description: "本文說明如何將掛接點資料夾路徑 (而非磁碟機代號) 指派給磁碟機。"
keywords: "虛擬化, 安全性, 惡意程式碼"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 01aaf73f694a6a7a9c516e4358f22dec0d1b4bc4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="assign-a-mount-point-folder-path-to-a-drive"></a>將掛接點資料夾路徑指派給磁碟機

> **適用於：**Windows 10、Windows 8.1、Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用 [磁碟管理] 將掛接點資料夾路徑 (而非磁碟機代號) 指派給磁碟機。 掛接點資料夾路徑只能在基本或動態 NTFS 磁碟區上的空資料夾使用。

## <a name="assigning-a-mount-point-folder-path-to-a-drive"></a>將掛接點資料夾路徑指派給磁碟機

-   [使用 Windows 介面](#BKMK_WINUI)
-   [使用命令列](#BKMK_CMD)

> [!NOTE]
> 您必須至少是**備份操作員**或**系統管理員**群組的成員，才能完成這些步驟。

**若要使用 Windows 介面將掛接點資料夾路徑指派給磁碟機：**
<a id="BKMK_WINUI"></a>

1.  在 [磁碟管理] 中，以滑鼠右鍵按一下您要指派掛接點資料夾路徑的磁碟分割或磁碟區。 
2. 按一下 **\[變更磁碟機代號及路徑\]**，然後按一下 **\[新增\]**。 
3. 按一下**\[掛在下列空的 NTFS 資料夾上\]**。
4. 輸入 NTFS 磁碟區的空資料夾路徑，或按一下 **\[瀏覽\]** 來尋找資料夾。

<a id="BKMK_CMD"></a>
#### <a name="to-assign-a-mount-point-folder-path-to-a-drive-using-a-command-line"></a>若要使用命令列將掛接點資料夾路徑指派給磁碟機
1.  開啟命令提示字元並輸入 `diskpart`。

2.  在 **DISKPART** 提示中輸入 `list volume`，並記下您要指派路徑的磁碟區編號。

3.  在 **DISKPART** 提示中輸入 `select volume <volumenumber>`。 

4. 選取您要指派路徑的簡單磁碟區 *volumenumber*。

5.  在 **DISKPART** 提示中輸入 `assign [mount=<path>]`。

#### <a name="to-remove-a-mount-point-folder-path-to-a-drive"></a>若要移除磁碟機的掛接點資料夾路徑

-   若要掛接點資料夾路徑，請按一下路徑，再按一下 **\[移除\]**。

<br />

| 值 | 描述 |
| --- | --- |
| <p>**list volume**</p> | <p>顯示所有磁碟上的基本和動態磁碟區。</p> |
| <p>**select volume**</p>        | <p>選取指定的磁碟區 (其中 <em>volumenumber</em> 是磁碟區編號)，並讓它成為焦點。 如果沒有指定磁碟區，**select** 會命令列出焦點所在的目前磁碟區。 您可以用編號、磁碟機代號或掛接點資料夾路徑來指定磁碟區。 在基本磁碟上，選取磁碟區也會讓對應的磁碟分割成為焦點。</p>|
| <p>**assign**</p> | <p><ul><li> 將磁碟機代號或掛接點資料夾路徑指派給焦點所在的磁碟區。 如果沒有指定磁碟機代號或掛接點資料夾路徑，則指派下一個可用的磁碟機代號。 如果磁碟機代號或掛接點資料夾路徑已在使用中，就會產生錯誤。</li> </p> <p><li>您可以使用 **assign** 命令，變更與抽取式磁碟機關聯的磁碟機代號。</li> </p><p><li> 您無法將磁碟機代號指派給開機磁碟區或包含分頁檔的磁碟區。 此外，您也無法將磁碟機代號指派給原始設備製造商 (OEM) 磁碟分割、EFI 系統磁碟分割，或基本資料磁碟分割以外的任何 GPT 磁碟分割。</p></li></ul> |
| <p>**mount=** <em>path</em></p> | <p>指定掛接磁碟位置所在空的現有 NTFS 資料夾。</p>  |

## <a name="additional-considerations"></a>其他考量

-   如果您正在管理本機或遠端電腦，您可以瀏覽該電腦上的 NTFS 資料夾。
-   掛接點資料夾路徑只能在基本或動態 NTFS 磁碟區上的空資料夾使用。
-   若要修改掛接點資料夾路徑，請先將該路徑移除，然後再使用新的位置建立新的資料夾路徑。 您無法直接修改掛接點資料夾路徑。
-   指派掛接點資料夾路徑給磁碟機時，請使用 **\[事件檢視器\]**，檢查系統記錄檔是否有任何指出掛接點資料夾路徑失敗的叢集服務錯誤或警告。 這些錯誤會在 **\[來源\]** 欄中列為 **\[ClusSvc\]**，並在 **\[類別\]** 欄中列為 **\[實體磁碟資源\]**。
-   您也可以使用 [mountvol](http://go.microsoft.com/fwlink/?linkid=64111) 命令建立掛接磁碟。

## <a name="see-also"></a>請參閱
-   [命令列語法標記法](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


