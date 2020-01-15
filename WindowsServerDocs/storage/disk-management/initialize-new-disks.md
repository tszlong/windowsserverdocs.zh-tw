---
title: 將新磁碟初始化
description: 如何使用 [磁碟管理] 將新磁碟初始化，以準備開始使用。 也包含問題的疑難排解連結。
ms.date: 12/20/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c2cb88d5b30be28a8ab7709e3a3908ce82ae8408
ms.sourcegitcommit: bfe9c5f7141f4f2343a4edf432856f07db1410aa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75352352"
---
# <a name="initialize-new-disks"></a>將新磁碟初始化

> **適用於：** Windows 10、Windows 8.1、Windows 7、Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您將全新的磁碟新增至電腦，而且它未顯示在檔案總管中，您可能需要[新增磁碟機代號](change-a-drive-letter.md)，或將它初始化再使用它。 您只能將尚未格式化的磁碟機初始化。 將磁碟初始化會清除磁碟上的所有內容並準備好供 Windows 使用，在這之後，您可以將它格式化，再將檔案儲存至其中。

> [!WARNING]
> 如果您的磁碟上已有重要檔案，請勿將它初始化 - 您將會遺失所有檔案。 相反地，建議針對磁碟進行疑難排解，看看是否可以讀取檔案 - 請參閱[磁碟的狀態為未初始化或磁碟已完全遺失](troubleshooting-disk-management.md#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps)。

## <a name="to-initialize-new-disks"></a>將新磁碟初始化

以下說明如何使用 [磁碟管理] 將新磁碟初始化。 如果您偏好使用 PowerShell，請改用 [initialize-disk](https://docs.microsoft.com/powershell/module/storage/initialize-disk) Cmdlet。

1. 使用系統管理員權限開啟 [磁碟管理]。
 
    若要執行這項操作，請在工作列的搜尋方塊中鍵入**磁碟管理**，選取並按住 (或以滑鼠右鍵按一下) [磁碟管理]  ，然後選取 [以系統管理員身分執行]   > [是]  。 如果無法以系統管理員身分將它開啟，請改鍵入**電腦管理**，然後移至 [儲存體]   > [磁碟管理]  。
1. 在 [磁碟管理] 中，以滑鼠右鍵按一下您要初始化的磁碟，然後按一下 [初始化磁碟]  (如下所示)。 如果磁碟列為「離線」  ，請先以滑鼠右鍵按一下它，然後選取 [線上]  。

     請注意，某些 USB 磁碟機沒有初始化選項，只有格式化選項和[磁碟機代號](change-a-drive-letter.md)。

    ![顯示未格式化磁碟的 [磁碟管理]，並顯示 [初始化磁碟] 捷徑功能表](media/uninitialized-disk.PNG)
2. 在 [初始化磁碟]  對話方塊中 (如下所示)，檢查以確定選取了正確的磁碟，然後按一下 [確定]  接受預設磁碟分割樣式。 如果您需要變更磁碟分割樣式 (GPT 或 MBR)，請參閱[關於磁碟分割樣式 - GPT 和 MBR](#about-partition-styles---gpt-and-mbr)。

     磁碟狀態會短暫變更為 [正在初始化]  ，再變更為 [線上]  狀態。 如果因某些原因而導致初始化失敗，請參閱[磁碟的狀態為未初始化或磁碟已完全遺失](troubleshooting-disk-management.md#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps)。

    ![選取 GPT 磁碟分割樣式的 [初始化磁碟] 對話方塊](media/initialize-disk.PNG)

3. 選取並按住 (或以滑鼠右鍵按一下) 磁碟機上未配置的空間，然後選取 [新增簡單磁碟區]  。
4. 選取 [下一步]  ，指定磁碟區的大小 (您可能會想要使用預設值，而使用整個磁碟機)，然後選取 [下一步]  。
5. 指定您要指派給磁碟區的磁碟機代號，然後選取 [下一步]  。
6. 指定您要使用的檔案系統 (通常是 NTFS)，選取 [下一步]  ，然後選取 [完成]  。

## <a name="about-partition-styles---gpt-and-mbr"></a>關於磁碟分割樣式 - GPT 和 MBR

磁碟可以分割成多個區塊，稱為磁碟分割。 每個磁碟分割都必須具有磁碟分割樣式 GPT 或 MBR，即使只有一個磁碟分割也一樣。 Windows 使用磁碟分割樣式來了解如何存取磁碟上的資料。

雖然這聽起或許不太有趣，但重點是，今日通常不需要擔心磁碟分割樣式，Windows 會自動使用適當的磁碟類型。

大多數電腦針對硬碟和 SSD 使用 GUID 磁碟分割表格 (GPT) 磁碟類型。 GPT 更穩固且允許大於 2 TB 的磁碟區。 32 位元電腦、舊版電腦和抽取式磁碟機 (例如記憶卡) 會使用舊版主開機記錄 (MBR) 磁碟類型。

若要在 MBR 與 GPT 之間轉換磁碟，您必須先從磁碟刪除所有磁碟區，以清除磁碟上的所有內容。 如需詳細資訊，請參閱[將 MBR 磁碟轉換為 GPT 磁碟](change-an-mbr-disk-into-a-gpt-disk.md)或[將 GPT 磁碟轉換為 MBR 磁碟](change-a-gpt-disk-into-an-mbr-disk.md)。