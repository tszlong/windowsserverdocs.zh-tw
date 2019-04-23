---
title: 檔案伺服器資源管理員 (FSRM) 概觀
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 5/14/2018
description: 檔案伺服器資源管理員 (FSRM) 是一種工具，可讓您管理和分類的 Windows Server 檔案伺服器上的資料。
ms.openlocfilehash: 107d08f247fc56720ccc3d11a3db88c77377257c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870719"
---
# <a name="file-server-resource-manager-fsrm-overview"></a>檔案伺服器資源管理員 (FSRM) 概觀

> 適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2

檔案伺服器資源管理員 (FSRM) 是 Windows Server 中的角色服務，可讓您管理和分類儲存在檔案伺服器上的資料。 您可以使用檔案伺服器資源管理員會自動分類檔案、 執行根據這些分類的工作、 設定配額的資料夾，以及建立監視儲存體使用量的報告。

它也是小型的點，但我們[新增能力以停用變更日誌](#whats-new)1803年版是 Windows Server 中。

## <a name="features"></a>功能

檔案伺服器資源管理員包含下列功能：

-   [配額管理](quota-management.md)可讓您限制磁碟區或資料夾，允許的空間，而且它們可以自動套用到磁碟區建立的新資料夾。 您也可以定義能夠套用至新磁碟區或資料夾的配額範本。  
-   [檔案分類基礎結構](classification-management.md)透過自動化分類程序，以便更有效地管理您的資料提供深入了解您的資料。 您可以分類檔案，並以該分類為基礎套用原則。 這些原則的範例包括限制檔案存取、檔案加密以及檔案到期的動態存取控制。 您可以使用檔案分類規則自動分類檔案，或修改所選檔案或資料夾的屬性來手動分類檔案。
-   [檔案管理工作](file-management-tasks.md)可讓您根據分類檔案套用條件性原則或動作。 檔案管理工作的條件包括檔案位置、分類屬性、檔案建立日期、檔案上次修改日期，或檔案上次存取時間。 檔案管理工作可以採取的動作包括使檔案到期、加密檔案或執行自訂命令的能力。
-   [檔案檢測管理](file-screening-management.md)能夠協助您控制的檔案類型的檔案伺服器上，可以儲存該使用者。 您可以限制可以儲存在共用檔案上的副檔名。 例如，您可以建立一個檔案檢測，不允許在檔案伺服器的個人共用資料夾中儲存副檔名為 MP3 的檔案。
-   [存放裝置報告](storage-reports-management.md)幫助您識別磁碟使用量和資料的分類方式的趨勢。 您也可以監視選取的使用者群組是否嘗試儲存未授權的檔案。  
  
包含與檔案伺服器資源管理員的功能可設定及管理使用檔案伺服器資源管理員應用程式，或使用 Windows PowerShell。
  
> [!IMPORTANT]
>  檔案伺服器資源管理員只支援以 NTFS 檔案系統格式化的磁碟區。 不支援彈性檔案系統。  
  
## <a name="practical-applications"></a>實際應用  
 檔案伺服器資源管理員包含下列實際應用：  
  
-   搭配使用檔案分類基礎結構和動態存取控制案例來建立一個原則，該原則可以根據檔案伺服器的檔案分類方式，授與檔案與資料夾存取權。  
  
-   建立一個檔案分類規則，用來將任何包含至少 10 個身份證號碼的檔案，標記成具有個人識別資訊的檔案。  
  
-   將過去 10 年未曾修改過的任何檔案視為已到期檔案。  
  
-   在每個使用者的主目錄建立 200 MB 的配額，並在已使用 180 MB 時予以通知。  
  
-   不允許在個人共用資料夾儲存任何音樂檔。  
  
-   將報告排程在每個星期天午夜執行，並產生一份過去兩天內最新存取的檔案清單。 這麼做可以幫助您判斷週末存放活動，並據此規劃您的伺服器停機時間。  

## <a name="whats-new"></a>最新消息-防止 FSRM 建立變更日誌

從 Windows Server，版本 1803，您可以現在防止檔案伺服器資源管理員服務在服務啟動時，磁碟區上建立變更日誌 （也稱為 USN 日誌）。 這可以節省一些的空間，每個磁碟區，但會停用即時的檔案分類。

如需較舊的新功能，請參閱[什麼是檔案伺服器資源管理員的新](https://technet.microsoft.com/library/dn383587.aspx)。

若要防止檔案伺服器資源管理員建立部分或所有磁碟區上的變更日誌，在服務啟動時，請使用下列步驟： 

1. 停止 SRMSVC 服務。 例如，開啟 以系統管理員的 PowerShell 工作階段，然後輸入`Stop-Service SrmSvc`。
2. 刪除您想要使用 fsutil 命令，節省的空間的磁碟區，USN 日誌： 

      ```
      fsutil usn deletejournal /d <VolumeName>
      ```
    例如：`fsutil usn deletejournal /d c:`

3. 開啟登錄編輯程式，例如，輸入`regedit`相同的 PowerShell 工作階段中。
4. 瀏覽至下列機碼：**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings**
5. 若要選擇性地略過將變更日誌建立整部伺服器 （如果您想要停用它只能在特定磁碟區上的這個步驟請略過）：
    1. 以滑鼠右鍵按一下**設定**鍵，然後選取**新增** > **DWORD （32 位元） 值**。 
    1. 此值命名為`SkipUSNCreationForSystem`。
    1. 將值設為**1** （以十六進位）。
6. 若要選擇性地略過特定的磁碟區的變更日誌建立：
    1. 取得您想要跳過使用的磁碟區路徑`fsutil volume list`命令或下列 PowerShell 命令：
        ```PowerShell
        Get-Volume | Format-Table DriveLetter,FileSystemLabel,Path
        ```
       以下是範例輸出：

       ```
        DriveLetter FileSystemLabel Path
        ----------- --------------- ----
                    System Reserved \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        C                           \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
       ```
    2. 傳回在登錄編輯器中，以滑鼠右鍵按一下**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings**鍵，然後選取**新增** > **多字串值**。
    3. 此值命名為`SkipUSNCreationForVolumes`。
    4. 輸入每個磁碟區所在您略過建立變更日誌，將每個路徑放在個別行上的路徑。 例如: 

        ```
        \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
        ```

        > [!NOTE] 
        > 登錄編輯程式可能會告訴您，它會移除空的字串，顯示您可以放心地忽略此警告：*REG_MULTI_SZ 類型的資料不能包含空字串。登錄編輯程式將會移除所有找到的空字串。*

7. 啟動 SRMSVC 服務。 例如，在 PowerShell 工作階段中輸入`Start-Service SrmSvc`。



## <a name="see-also"></a>另請參閱

- [動態存取控制](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 