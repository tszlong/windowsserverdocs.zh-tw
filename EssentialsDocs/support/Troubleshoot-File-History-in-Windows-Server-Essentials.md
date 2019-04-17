---
title: "在 Windows Server Essentials 檔案歷史的疑難排解"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 34442565b54b089064c1fa19317a24f591e44fda
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="troubleshoot-file-history-in-windows-server-essentials"></a>在 Windows Server Essentials 檔案歷史的疑難排解

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集 
  
## <a name="troubleshoot-issues-with-user-file-history-backups"></a>疑難排解使用者檔案歷史備份的問題  
 下列問題可能會發生時管理檔案歷史備份的使用者或電腦已加入到執行 Windows Server Essentials 的伺服器。  
  
### <a name="file-history-data-is-not-automatically-deleted"></a>不會自動刪除檔案歷史資料  
 歷史檔案的資料可能如果不會自動取得刪除：  
  
-   刪除帳號，您選擇不 delete s 檔案歷史資料、帳號，並加入手動 delete 資料。  
  
-   當您嘗試 delete 檔案歷史資料時、檔案歷史資料是使用其他處理程序。  
  
 若要修正這個問題的相關，您必須手動 delete 使用下列程序檔案歷史：  
  
####  <a name="BKMK_manuallyDelete"></a>若要手動 delete 檔案歷史備份的使用者或電腦  
  
1.  以系統管理員身分登入伺服器。  
  
2.  系統管理員身分執行檔案總管]。  
  
3.  瀏覽到檔案的備份歷史的資料夾。 預設位置為 C:\ServerFolders\File 歷史備份。  
  
4.  Delete 儲存備份檔案歷史的共用的資料夾：  
  
    -   Delete 使用者的檔案歷史，以 delete s 使用者名稱的檔案歷史備份子資料夾。  
  
    -   Delete 檔案歷史的電腦，以 delete 電腦名稱的檔案歷史備份子資料夾。 例如，如果她可以開始使用她新的膝上型電腦，< MyComputer02\ > 之後，使用者淘汰 < MyComputer01\ > 想 delete C:\ServerFolders\File 歷史 Backups\\ < MyAccount\ > \\ < MyComputer01\ > 之後您確認使用者她已經傳送的所有檔案與資料夾到她新的膝上型電腦，而且有不需要的檔案歷史未來。  
  
### <a name="cannot-apply-file-history-setting-to-a-new-user"></a>無法將檔案歷史設定套用到新的使用者  
 若要新增的使用者名稱是已被從 Windows Server Essentials 使用者的使用者名稱相同的新的使用者，Windows Server Essentials 嘗試來建立資料夾，以儲存新使用者的檔案歷史時的新使用者的檔案歷史設定可能會失敗因為命名衝突。 若要修正這個問題的相關，您可以重新命名刪除使用者的檔案歷史資料夾。  
  
##### <a name="to-locate-user-file-history-on-the-server"></a>若要在伺服器上尋找使用者檔案歷史  
  
1.  以系統管理員身分登入伺服器。  
  
2.  Windows Server Essentials 儀表板，請按一下**存放裝置**。  
  
3.  在**資料夾伺服器**索引標籤上，記下該檔案的備份歷史資料夾的位置。 預設位置為 %SystemDrive%\ServerFolders\File 歷史 Backups\\。  
  
##### <a name="to-resolve-file-history-issues-for-a-new-user-with-a-name-conflict"></a>修正檔案歷史的相關問題，有新的使用者名稱衝突使用  
  
1.  以系統管理員身分登入伺服器。  
  
2.  系統管理員身分執行檔案總管]。  
  
3.  瀏覽到檔案的備份歷史的資料夾。 預設位置為 C:\ServerFolders\File 歷史備份。  
  
     歷史檔案的備份] 資料夾已經每個使用者 account 已加入到 Windows Server Essentials 的子資料夾。 歷史姓氏使用者的檔案，例如會儲存檔案歷史 Backups\JohnSmith 的子資料夾中。  
  
4.  重新命名您刪除，例如，使用者的子資料夾**<*的使用者名稱*> _Deleted * *。 如果您不再需要的使用者的檔案歷史，您可以 delete 資料夾。  
  

5.  您現在可以新增新的使用者。 指示，請查看 [新增使用者帳號嗎？在[管理帳號](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>已移除使用者帳號，但仍使用者的檔案歷史  
 有時候網路系統管理員可以選擇要移除的使用者或電腦的伺服器，但保留檔案歷史備份未來使用。 當您不再需要的檔案歷史時，移除伺服器上的共用資料夾使用者或電腦的檔案歷史備份資料夾。 若要這樣做，請查看[以手動方式 delete 的使用者或電腦歷史檔案備份到](Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete)。  

5.  您現在可以新增新的使用者。 指示，請查看 [新增使用者帳號嗎？在[管理帳號](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>已移除使用者帳號，但仍使用者的檔案歷史  
 有時候網路系統管理員可以選擇要移除的使用者或電腦的伺服器，但保留檔案歷史備份未來使用。 當您不再需要的檔案歷史時，移除伺服器上的共用資料夾使用者或電腦的檔案歷史備份資料夾。 若要這樣做，請查看[以手動方式 delete 的使用者或電腦歷史檔案備份到](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete)。  

  
## <a name="see-also"></a>也了  
  
-   [管理 Client 備份](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)  
  

-   [Windows Server Essentials 的支援](Support-Windows-Server-Essentials.md)

-   [Windows Server Essentials 的支援](../support/Support-Windows-Server-Essentials.md)

