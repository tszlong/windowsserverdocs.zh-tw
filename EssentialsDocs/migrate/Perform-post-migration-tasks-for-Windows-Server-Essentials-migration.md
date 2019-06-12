---
title: 執行 Windows Server Essentials migration1 移轉後工作
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2d236a4-0d62-4961-9d1f-332054e06f6d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 93d07938435ab1ce7686b1960974696582a2924c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432658"
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>執行 Windows Server Essentials migration1 移轉後工作

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

下列工作可協助您完成目的地伺服器設定，其中一些設定與來源伺服器上的設定相同。 您可能已在移轉過程中停用來源伺服器上其中某些設定，這樣它們就不會移轉到目的地伺服器。 或者，它們是您可能會想執行的選擇性設定步驟。  
  

-   [刪除來源伺服器的 DNS 項目](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [共用的業務和其他應用程式資料資料夾](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [修正移轉後用戶端電腦問題](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [提供內建的 Administrators 群組以批次工作登入的權限](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

-   [刪除來源伺服器的 DNS 項目](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [共用的業務和其他應用程式資料資料夾](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [修正移轉後用戶端電腦問題](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [提供內建的 Administrators 群組以批次工作登入的權限](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

  
##  <a name="BKMK_DeleteDNSEntries"></a> 刪除來源伺服器的 DNS 項目  
 在您解除委任來源伺服器之後，網域名稱服務 (DNS) 伺服器可能仍然會包含指向來源伺服器的項目。 請刪除這些 DNS 項目。  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>若要刪除指向來源伺服器的 DNS 項目  
  
1.  在目的地伺服器上，開啟 [DNS 管理員]  。  
  
2.  在 [DNS 管理員] 中，以滑鼠右鍵按一下伺服器名稱，按一下 [內容]  ，然後按一下 [轉寄站]  索引標籤。  
  
3.  判斷轉寄站清單中是否有一個項目指向來源伺服器。 如果有，請按一下 [編輯]  ，然後在 [編輯轉寄站]  視窗中刪除該項目。  
  
4.  在 [DNS 管理員]  中，展開伺服器名稱，然後再展開 [正向對應區域]  。  
  
5.  針對每個正向對應區域，以滑鼠右鍵按一下區域，按一下 [內容]  ，然後按一下 [名稱伺服器]  索引標籤。  
  
6.  在 [名稱伺服器]  方塊中按一下指向來源伺服器的項目，按一下 [移除]  ，然後按一下 [確定]  。  
  
7.  重複步驟 5 和 6，直到移除指向來源伺服器的所有項目為止。  
  
8.  按一下 **[確定]** 關閉 **[內容]** 視窗。  
  
9. 在 **[DNS 管理員]** 主控台中，展開 **[反向對應區域]** 。  
  
10. 重複步驟 6 到 9，移除所有指向來源伺服器的反向對應區域。  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a> 共用的業務和其他應用程式資料資料夾  
 您必須對複製到目的地伺服器的企業營運應用程式與其他應用程式資料資料夾，設定共用資料夾權限和 NTFS 權限。 共用的資料夾設定權限之後，會顯示在 Windows Server Essentials 儀表板中**儲存體**一節。  
  
 若使用登入指令碼將磁碟機對應至共用資料夾，您必須更新指令碼為對應至目的地伺服器上的磁碟機。  
  
##  <a name="BKMK_FixClientComputerIssuesAfterMigrating"></a> 修正移轉後用戶端電腦問題  
 如果您從 Windows Small Business Server 2003 Premium Edition 與 Microsoft Internet Security 和 Acceleration (ISA) Server 安裝移轉到 Windows Server Essentials，用戶端電腦在網路上的還有 Microsoft Firewall Client 和網際網路總管設定為使用 proxy 伺服器。  
  
 這會導致用戶端電腦的連線問題，因為 Proxy 伺服器已不存在。 如果沒有設定不同的 proxy 伺服器，用戶端電腦繼續使用 proxy 伺服器執行 Windows SBS 2003 的伺服器。 為修正此問題，您必須將 Internet Explorer 重新設定為不使用 Proxy 伺服器或使用新的 Proxy 伺服器。  
  
#### <a name="to-reconfigure-internet-explorer"></a>重新設定 Internet Explorer  
  
1.  在 Internet Explorer 中，按一下 [工具]  ，然後按一下 [網際網路選項]  。  
  
2.  依序按一下 [連線]  索引標籤、[區域網路設定]  ，然後執行下列其中一項：  
  
    -   如果您的網路上未使用 Proxy 伺服器，請清除 [區域網路 (LAN) 設定]  對話方塊中的核取方塊。  
  
    -   如果您想在網路上使用新的 Proxy 伺服器：  
  
        1.  在 [區域網路 (LAN) 設定]  對話方塊中，清除 [自動設定]  區段中的核取方塊。  
  
        2.  在 [Proxy 伺服器]  區段中，確認兩個核取方塊都已選取。  
  
        3.  在 [位址]  方塊中，輸入 Proxy 伺服器的完整網域名稱 (FQDN)。  
  
        4.  在 [連接埠]  方塊中，輸入 **80**。  
  
3.  按兩次 **[確定]** 。  
  
4.  瀏覽至網站以確保連線設定正確。  
  
##  <a name="BKMK_AdminGroup"></a> 提供內建的 Administrators 群組以批次工作登入的權限  
 您將現有的 Windows Small Business Server 2003 網域移轉到 Windows Server Essentials 之後，您應該提供內建的 Administrators 群組以批次工作登入權限。 確認內建的 Administrators 群組仍具有以批次工作登入目的地伺服器的權限。 系統管理員需要此權限，才能在未登入的情況下於目的地伺服器上管理警示。  
  
#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>提供內建的 Administrators 群組以批次工作登入的權限  
  
1. 在目的地伺服器上，開啟 [群組原則管理]  系統管理工具。  
  
2. 在 **群組原則管理**主控台樹狀目錄中，展開**樹系：** *< ServerName\>* ，展開 網域，然後再展開 您的伺服器。  
  
3. 展開 網域控制站  ，以滑鼠右鍵按一下 預設網域控制站原則  ，然後按一下 編輯  。  
  
4. 在 **群組原則管理編輯器**，按一下**Default Domain Controllers Policy**<em>< ServerName\></em>**原則**，然後展開**電腦設定**。  
  
5. 展開 [原則]  、[Windows 設定]  ，然後展開 [安全性設定]  。  
  
6. 在 [安全性設定]  樹狀目錄，展開 [本機原則]  ，然後按一下 [使用者權限指派]  。  
  
7. 在 [結果] 窗格中，以滑鼠右鍵按一下 [以批次工作登入]  ，然後按一下 [內容]。  
  
8. 在 [以批次工作內容登入]  頁面，按一下 [新增使用者或群組]  。  
  
9. 在 [新增使用者或群組]  對話方塊中，按一下 [瀏覽]  。  
  
10. 在 [選取使用者、電腦或群組]  對話方塊中，輸入 **Administrators**。  
  
11. 按一下 [檢查名稱]  以確認顯示內建的 Administrators 群組，然後按三次 [確定]  以儲存設定。  
  
## <a name="see-also"></a>另請參閱  
  

-   [從 Windows SBS 2003 移轉](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [移轉伺服器資料到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [從 Windows SBS 2003 移轉](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [移轉伺服器資料到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

