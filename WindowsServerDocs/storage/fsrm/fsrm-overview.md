---
title: 檔案伺服器資源管理員 (FSRM) 概觀
ms.prod: windows-server
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 5/14/2018
description: 檔案伺服器 Resource Manager （FSRM）是一種工具，可讓您管理和分類 Windows Server 檔案伺服器上的資料。
ms.openlocfilehash: 719176307afc320ad676fd1acfc07ad9d15920cf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394174"
---
# <a name="file-server-resource-manager-fsrm-overview"></a>檔案伺服器資源管理員 (FSRM) 概觀

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server （半年通道）、 

檔案伺服器資源管理員 (FSRM) 是 Windows Server 中的角色服務，可讓您管理和分類儲存在檔案伺服器上的資料。 您可以使用 [檔案伺服器 Resource Manager] 自動分類檔案、根據這些分類執行工作、設定資料夾配額，以及建立監視儲存體使用量的報表。

這是個小點，但我們也新增了在 Windows Server 版本1803中[停用變更日誌的功能](#whats-new)。

## <a name="features"></a>功能

檔案伺服器資源管理員包含下列功能：

-   [配額管理](quota-management.md)可讓您限制磁片區或資料夾允許使用的空間，而且可以自動套用到磁片區上建立的新資料夾。 您也可以定義能夠套用至新磁碟區或資料夾的配額範本。  
-   檔案[分類基礎結構](classification-management.md)透過自動化分類程式來提供資料的深入解析，讓您可以更有效率地管理您的資料。 您可以分類檔案，並以該分類為基礎套用原則。 這些原則的範例包括限制檔案存取、檔案加密以及檔案到期的動態存取控制。 您可以使用檔案分類規則自動分類檔案，或修改所選檔案或資料夾的屬性來手動分類檔案。
-   檔案[管理](file-management-tasks.md)工作可讓您根據檔案的分類，將條件性原則或動作套用至檔案。 檔案管理工作的條件包括檔案位置、分類屬性、檔案建立日期、檔案上次修改日期，或檔案上次存取時間。 檔案管理工作可以採取的動作包括使檔案到期、加密檔案或執行自訂命令的能力。
-   檔案檢測[管理](file-screening-management.md)可協助您控制使用者可以儲存在檔案伺服器上的檔案類型。 您可以限制可以儲存在共用檔案上的副檔名。 例如，您可以建立一個檔案檢測，不允許在檔案伺服器的個人共用資料夾中儲存副檔名為 MP3 的檔案。
-   [存放裝置報告](storage-reports-management.md)可協助您識別磁片使用量的趨勢，以及資料的分類方式。 您也可以監視選取的使用者群組是否嘗試儲存未授權的檔案。  
  
檔案伺服器 Resource Manager 隨附的功能可以使用檔案伺服器 Resource Manager 應用程式，或使用 Windows PowerShell 來進行設定和管理。
  
> [!IMPORTANT]
>  檔案伺服器資源管理員只支援以 NTFS 檔案系統格式化的磁碟區。 不支援彈性檔案系統。  
  
## <a name="practical-applications"></a>實際應用  
 檔案伺服器資源管理員包含下列實際應用：  
  
-   搭配使用檔案分類基礎結構和動態存取控制案例來建立一個原則，該原則可以根據檔案伺服器的檔案分類方式，授與檔案與資料夾存取權。  
  
-   建立一個檔案分類規則，用來將任何包含至少 10 個身份證號碼的檔案，標記成具有個人識別資訊的檔案。  
  
-   將過去 10 年未曾修改過的任何檔案視為已到期檔案。  
  
-   為每個使用者的主目錄建立 200 mb 的配額，並在使用 180 mb 時通知他們。  
  
-   不允許在個人共用資料夾儲存任何音樂檔。  
  
-   將報告排程在每個星期天午夜執行，並產生一份過去兩天內最新存取的檔案清單。 這麼做可以幫助您判斷週末存放活動，並據此規劃您的伺服器停機時間。  

## <a name="whats-new"></a>新功能-防止 FSRM 建立變更日誌

從 Windows Server 版本1803開始，您現在可以在服務啟動時，防止檔案伺服器 Resource Manager 服務在磁片區上建立變更日誌（也稱為 USN 日誌）。 這可以節省每個磁片區上的空間，但會停用即時檔案分類。

如需舊版的新功能，請參閱[檔案伺服器 Resource Manager 的新](https://technet.microsoft.com/library/dn383587.aspx)功能。

若要防止檔案伺服器 Resource Manager 在服務啟動時于部分或所有磁片區上建立變更日誌，請使用下列步驟： 

1. 停止 SRMSVC 服務。 例如，以系統管理員身分開啟 PowerShell 會話，然後輸入`Stop-Service SrmSvc`。
2. 使用 fsutil 命令，刪除您想要用來節省空間的磁片區 USN 日誌： 

      ```
      fsutil usn deletejournal /d <VolumeName>
      ```
    例如：`fsutil usn deletejournal /d c:`

3. 開啟 [登錄編輯程式]，例如，在`regedit`相同的 PowerShell 會話中輸入。
4. 流覽至下列機碼：**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings**
5. 選擇性地略過整部伺服器的變更日誌建立（如果您只想要在特定磁片區上停用，請略過此步驟）：
    1. 在 [**設定**] 機碼上按一下滑鼠右鍵，然後選取 [**新增** >  **DWORD （32-位）值**]。 
    1. 將值`SkipUSNCreationForSystem`命名為。
    1. 將值設為**1** （十六進位）。
6. 若要選擇性地略過特定磁片區的變更日誌建立：
    1. 使用`fsutil volume list`命令或下列 PowerShell 命令來取得您想要略過的磁片區路徑：
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
    2. 回到 [登錄編輯程式]，以滑鼠右鍵按一下 [ **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings** ] 機碼，然後選取 [**新增** > **多字串值**]。
    3. 將值`SkipUSNCreationForVolumes`命名為。
    4. 輸入您略過建立變更日誌之每個磁片區的路徑，並將每個路徑放在不同的行上。 例如:

        ```
        \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
        ```

        > [!NOTE] 
        > 登錄編輯程式可能會告訴您它已移除空字串，並顯示此警告，您可以放心地忽略：類型為 REG_MULTI_SZ 的 @no__t 0Data 不能包含空字串。登錄編輯程式將會移除所有找到的空字串。*

7. 啟動 SRMSVC 服務。 例如，在 PowerShell 會話中輸入`Start-Service SrmSvc`。



## <a name="see-also"></a>另請參閱

- [動態存取控制](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 