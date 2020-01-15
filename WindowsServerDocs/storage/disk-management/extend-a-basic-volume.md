---
title: 延伸基本磁碟區
description: 您可以在 Windows 中的現有磁碟區上新增空間，並將其延伸至磁碟機上的可用空間中，但此做法僅限於可用空間中沒有磁碟區 (未配置)，並且緊接在您要延伸的磁碟區之後，而中間沒有其他的磁碟區的情況。 本文說明如何執行這項作業。
ms.date: 12/19/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c72b242437c4c308da77a25e06f3d76e4c65f480
ms.sourcegitcommit: bfe9c5f7141f4f2343a4edf432856f07db1410aa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75351890"
---
# <a name="extend-a-basic-volume"></a>延伸基本磁碟區

> **適用於：** Windows 10、Windows 8.1、Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用「磁碟管理」在現有的磁碟區上新增空間，並將其延伸至磁碟機上的可用空間中，但此做法僅限於可用空間中沒有磁碟區 (未配置)，並且緊接在您要延伸的磁碟區之後，而中間沒有其他的磁碟區的情況，如下圖所示。 要延伸的磁碟區也必須使用 NTFS 或 ReFS 檔案系統進行格式化。

:::image type="content" source="media/extend-volume-space-highlighted.png" alt-text="磁碟管理顯示磁碟區可延伸至的可用空間。":::

## <a name="to-extend-a-volume-by-using-disk-management"></a>使用磁碟管理延伸磁碟區

以下說明在磁碟區位於磁碟機上之後，如何將磁碟區延伸至可用空間中：

1. 使用系統管理員權限開啟 [磁碟管理]。

   執行此動作的簡易方法，是在工作列上的 [搜尋] 方塊中輸入**電腦管理**，選取並按住 (以滑鼠右鍵按一下) [電腦管理]  ，然後選取 [以系統管理員身分執行]   > [是]  。 在開啟 [電腦管理] 後，移至 [存放裝置]   > [磁碟管理]  。
2. 選取並按住 (或以滑鼠右鍵按一下) 您要延伸的磁碟區，然後選取 [延伸磁碟區]  。

   如果 [延伸磁碟區]  呈現為灰色，請確認下列事項：
    - 已使用系統管理員權限開啟「磁碟管理」或「電腦管理」
    - 緊接在磁碟區的後面 (右側) 有未配置的空間，如上圖所示。 如果未配置的空間與您要延伸的磁碟區之間有另一個磁碟區，您可以刪除該磁碟區及其上的所有檔案 (請務必先備份或移出任何重要檔案！)，使用可在不損毀資料的情況下移動磁碟區的非 Microsoft 磁碟分割應用程式，或略過磁碟區延伸作業，而改為在未配置的空間中建立個別磁碟區。
    - 磁碟區須使用 NTFS 或 ReFS 檔案系統進行格式化。 無法延伸其他檔案系統，因此您必須移出或備份磁碟區上的檔案，然後使用 NTFS 或 ReFS 檔案系統來格式化磁碟區。
    - 如果磁碟大於 2 TB，請務必使用 GPT 分割配置。 若要在磁碟上使用超過 2 TB，則必須使用 GPT 分割配置加以初始化。 若要轉換為 GPT，請參閱[將 MBR 磁碟變更為 GPT 磁碟](change-an-mbr-disk-into-a-gpt-disk.md)。
    - 如果仍然無法延伸磁碟區，請嘗試搜尋 [的 Microsoft 社群 - 檔案、資料夾與儲存體](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639)網站，如果找不到答案，請在該處張貼問題，讓 Microsoft 或其他社群成員協助您，或是[連絡 Microsoft 支援服務](https://support.microsoft.com/contactus/)。

3. 選取 [下一步]  ，然後在精靈的 [選取磁碟]  頁面上 (如下所示)，指定要延伸磁碟區的數量。 一般來說只要使用預設值即可，此時會使用所有的可用空間，但如果您想要在可用空間中建立其他磁碟區，則可以使用較小的值。

   :::image type="content" source="media/extend-volume-wizard.png" alt-text="延伸磁碟區精靈顯示要延伸以取得所有可用空間的磁碟區":::

4. 選取 [下一步]  ，然後選取 [完成]  ，以延伸磁碟區。

## <a name="to-extend-a-volume-by-using-powershell"></a>使用 PowerShell 延伸磁碟區

1. 選取並按住 (或以滑鼠右鍵按一下) [開始] 按鈕，然後選取 [Windows PowerShell (系統管理員)]。
2. 輸入下列命令，並在 *$drive_letter* 變數中指定您要延伸之磁碟區的磁碟機代號，將磁碟區大小調整為大小上限：

   ```PowerShell
   # Variable specifying the drive you want to extend
   $drive_letter = "C"

   # Script to get the partition sizes and then resize the volume
   $size = (Get-PartitionSupportedSize -DriveLetter $drive_letter)
   Resize-Partition -DriveLetter $drive_letter -Size $size.SizeMax
   ```

## <a name="see-slso"></a>另請參閱

- [Resize-Partition](https://docs.microsoft.com/powershell/module/storage/resize-partition)
- [Diskpart 延伸](https://docs.microsoft.com/windows-server/administration/windows-commands/extend)
