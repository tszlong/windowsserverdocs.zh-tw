---
title: 初始化新磁碟
description: 如何初始化新的磁碟使用磁碟管理，讓他們可供使用。 也包含連結的問題進行疑難排解。
ms.date: 10/24/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e009780d83220b528ba7dac6e2561be36e662f71
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192757"
---
# <a name="initialize-new-disks"></a>初始化新磁碟

> **適用於：** Windows 10，Windows 8.1、 Windows 7、 Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

如果新的磁碟新增至您的電腦，它不會顯示在 [檔案總管] 中，您可能需要[新增的磁碟機代號](change-a-drive-letter.md)，或將它初始化後才能使用它。 您只可以初始化尚未格式化的磁碟機。 初始化的磁碟清除它的所有項目，並準備好以供 Windows 之後，您可以格式化並儲存在其上的檔案。

> [!WARNING]
> 如果您的磁碟已經有您想知道它的檔案，不將它初始化，-將會遺失所有檔案。 取而代之的是我們建議疑難排解磁碟来知道是否您可以讀取檔案-請參閱[磁碟的狀態為 未初始化或磁碟已遺失完全](troubleshooting-disk-management.md#a-disks-status-is-not-initialized-or-the-disk-is-missing)。

## <a name="to-initialize-new-disks"></a>若要初始化新磁碟

以下是如何初始化新的磁碟使用磁碟管理。 如果您偏好使用 PowerShell，使用[初始化磁碟](https://docs.microsoft.com/powershell/module/storage/initialize-disk)cmdlet 改。

1. 您可以開啟 磁碟管理系統管理員權限。 
 
    若要這樣做，請在工作列上的 [搜尋] 方塊中輸入**磁碟管理**、 選取並保存 （或以滑鼠右鍵按一下）**磁碟管理**，然後選取**系統管理員身分執行** > **是**。 如果您無法以系統管理員身分開啟它，輸入**電腦管理**相反的然後移至**儲存體** > **磁碟管理**。
1. 在 磁碟管理，以滑鼠右鍵按一下您想要初始化，然後按一下 的磁碟**初始化磁碟**（如下所示）。 如果磁碟已列為*Offline*先以滑鼠右鍵按一下它，然後選取**線上**。

     請注意，有些 USB 磁碟機不需要初始化的選項，它們只是取得格式化並[磁碟機代號](change-a-drive-letter.md)。

    ![顯示未格式化的磁碟，以顯示 初始化磁碟 快顯功能表的 磁碟管理](media\uninitialized-disk.PNG)
2. 在 **初始化磁碟**（如下所示） 的對話方塊中，以確定已選取正確的磁碟，然後按一下核取**確定**接受預設分割區樣式。 如果您需要變更磁碟分割樣式 （GPT 或 MBR），請參閱[有關的磁碟分割樣式-GPT 和 MBR](#about-partition-styles---gpt-and-mbr)。

     磁碟狀態很快變為**Initializing**然後**線上**狀態。 如果初始化失敗，因為某些原因，請參閱[磁碟的狀態為 未初始化或磁碟已遺失完全](troubleshooting-disk-management.md#a-disks-status-is-not-initialized-or-the-disk-is-missing)。

    ![選取的 GPT 磁碟分割樣式初始化磁碟 對話方塊](media\initialize-disk.PNG)

## <a name="about-partition-styles---gpt-and-mbr"></a>有關磁碟分割樣式-GPT 和 MBR

磁碟可以分成多個區塊稱為 「 磁碟分割。 每個資料分割-即使只有一個-必須要有磁碟分割樣式-GPT 或 MBR。 Windows 會使用的磁碟分割樣式，來了解如何存取磁碟上的資料。

因為這可能不一樣有趣，重點是，這幾天，您通常不需要擔心磁碟分割樣式，-Windows 會自動使用適當的磁碟類型。

大部分的電腦的硬碟機和 Ssd 使用 GUID 磁碟分割表格 (GPT) 磁碟類型。 GPT 更穩固，可讓磁碟區大於 2 TB。 較舊的主開機記錄 (MBR) 磁碟類型會使用 32 位元電腦，較舊的電腦及記憶卡等抽取式磁碟機。

若要將磁碟從 MBR 或 GPT，您首先必須從磁碟中，刪除所有磁碟區清除磁碟上的所有項目。 如需詳細資訊，請參閱 <<c0> [ 轉換成 GPT 磁碟的 MBR 磁碟](change-an-mbr-disk-into-a-gpt-disk.md)，或[成 MBR 磁碟轉換為 GPT 磁碟](change-a-gpt-disk-into-an-mbr-disk.md)。