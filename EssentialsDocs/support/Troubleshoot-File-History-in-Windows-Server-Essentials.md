---
title: Windows Server Essentials 中的檔案歷程記錄問題疑難排解
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed062945-27e9-4572-b1bb-6c8cf1b9c2f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f080bed5714ae4426cc6d0ca8edb5fab2d3c65b2
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432468"
---
# <a name="troubleshoot-file-history-in-windows-server-essentials"></a>Windows Server Essentials 中的檔案歷程記錄問題疑難排解

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials 
  
## <a name="troubleshoot-issues-with-user-file-history-backups"></a>使用者檔案歷程記錄備份問題疑難排解  
 針對新增至執行 Windows Server Essentials 之伺服器的使用者或電腦管理檔案歷程記錄備份時，可能會發生下列問題。  
  
### <a name="file-history-data-is-not-automatically-deleted"></a>檔案歷程記錄資料不會自動刪除  
 檔案歷程記錄資料在下列情況可能不會自動刪除：  
  
- 刪除使用者帳戶時，您會選擇不刪除使用者帳戶的檔案歷程記錄資料，並選擇手動刪除資料。  
  
- 當您嘗試刪除檔案歷程記錄資料時，其他處理序正在使用檔案歷程記錄資料。  
  
  若要解決這個問題，您必須使用下列程序手動刪除檔案歷程記錄：  
  
####  <a name="BKMK_manuallyDelete"></a> 若要以手動方式刪除使用者或電腦的檔案歷程記錄備份  
  
1.  以系統管理員身分登入伺服器。  
  
2.  以系統管理員身分執行檔案總管。  
  
3.  巡覽至 File History Backups 資料夾。 預設位置為 C:\ServerFolders\File History Backups。  
  
4.  刪除儲存檔案歷程記錄備份的共用資料夾：  
  
    -   若要刪除的使用者檔案歷程記錄，請刪除具有使用者的名稱的檔案歷程記錄備份子資料夾。  
  
    -   若要刪除電腦的檔案歷程記錄，請刪除具有電腦名稱的檔案歷程記錄備份子資料夾。 例如，如果使用者已停用 < MyComputer01\>上新的膝上型電腦，開始工作之後 < MyComputer02\>，您會將刪除 C:\ServerFolders\File History Backups\\< MyAccount\> \\< MyComputer01\>她已所有檔案和資料夾都傳輸至新的膝上型電腦，並在未來有 「 檔案歷程記錄不需要與使用者驗證之後。  
  
### <a name="cannot-apply-file-history-setting-to-a-new-user"></a>無法將檔案歷程記錄設定套用至新使用者  
 如果您新增的使用者與已從 Windows Server Essentials 中刪除的使用者具有相同的使用者名稱，由於 Windows Server Essentials 嘗試建立用於儲存新使用者之檔案歷程記錄的資料夾時發生命名衝突，因此新使用者的檔案歷程記錄設定可能會失敗。 若要解決這個問題，您可以重新命名已刪除使用者的 File History 資料夾。  
  
##### <a name="to-locate-user-file-history-on-the-server"></a>在伺服器上找到使用者檔案歷程記錄  
  
1.  以系統管理員身分登入伺服器。  
  
2.  在 [Windows Server Essentials 儀表板] 上，按一下 [存放裝置]  。  
  
3.  在 [伺服器資料夾]  索引標籤上，記下 File History Backups 資料夾的位置。 預設位置為 %SystemDrive%\ServerFolders\File History Backups\\。  
  
##### <a name="to-resolve-file-history-issues-for-a-new-user-with-a-name-conflict"></a>解決發生名稱衝突之新使用者的檔案歷程記錄問題  
  
1.  以系統管理員身分登入伺服器。  
  
2.  以系統管理員身分執行檔案總管。  
  
3.  巡覽至 File History Backups 資料夾。 預設位置為 C:\ServerFolders\File History Backups。  
  
     檔案歷程記錄備份資料夾針對已新增至 Windows Server Essentials 的每個使用者帳戶，各包含一個子資料夾。 例如，使用者 John Smith 的檔案歷程記錄會儲存在子資料夾 File History Backups\JohnSmith 中。  
  
4.  重新命名已刪除，例如，使用者的子資料夾 **< *UserName*> _Deleted**。 如果您不再需要使用者的檔案歷程記錄，您可以刪除該資料夾。  
  

5.  您現在可以新增使用者。 如需指示，請參閱 < 新增使用者帳戶？在 [管理使用者帳戶](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>已移除使用者帳戶，但保留使用者的檔案歷程記錄  
 在某些情況下，網路管理員可選擇從伺服器移除使用者或電腦，但保留檔案歷程記錄備份供未來使用。 當您不再需要檔案歷程記錄時，請從伺服器上的共用資料夾，移除使用者或電腦的 File History Backups 資料夾。 若要這樣做，請參閱[以手動方式刪除使用者或電腦的檔案歷程記錄備份](Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete)。  

5. 您現在可以新增使用者。 如需指示，請參閱 < 新增使用者帳戶？在 [管理使用者帳戶](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>已移除使用者帳戶，但保留使用者的檔案歷程記錄  
 在某些情況下，網路管理員可選擇從伺服器移除使用者或電腦，但保留檔案歷程記錄備份供未來使用。 當您不再需要檔案歷程記錄時，請從伺服器上的共用資料夾，移除使用者或電腦的 File History Backups 資料夾。 若要這樣做，請參閱[以手動方式刪除使用者或電腦的檔案歷程記錄備份](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete)。  

  
## <a name="see-also"></a>另請參閱  
  
-   [管理用戶端備份](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)  
  

-   [支援 Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [支援 Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

