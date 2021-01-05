---
title: 執行 Windows Server Essentials migration1 的遷移後工作
description: 瞭解您在遷移 Windows Server Essentials 之後必須執行的遷移後工作。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: f2d236a4-0d62-4961-9d1f-332054e06f6d
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: cf96832b6731dd8680ad41a49cd1d260644ba103
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810655"
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>執行 Windows Server Essentials migration1 的遷移後工作

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

下列工作可協助您完成目的地伺服器設定，其中一些設定與來源伺服器上的設定相同。 您可能已在移轉過程中停用來源伺服器上其中某些設定，這樣它們就不會移轉到目的地伺服器。 或者，它們是您可能會想執行的選擇性設定步驟。


-   [刪除來源伺服器的 DNS 項目](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)

-   [共用企業營運應用程式與其他應用程式資料資料夾](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)

-   [修正移轉後的用戶端電腦問題](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)

-   [提供內建的 Administrators 群組以批次工作登入的權限](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)


##  <a name="delete-dns-entries-of-the-source-server"></a><a name="BKMK_DeleteDNSEntries"></a> 刪除來源伺服器的 DNS 專案
 在您解除委任來源伺服器之後，網域名稱服務 (DNS) 伺服器可能仍然會包含指向來源伺服器的項目。 請刪除這些 DNS 項目。

#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>若要刪除指向來源伺服器的 DNS 項目

1.  在目的地伺服器上，開啟 [DNS 管理員]。

2.  在 [DNS 管理員] 中，以滑鼠右鍵按一下伺服器名稱，按一下 [內容]，然後按一下 [轉寄站] 索引標籤。

3.  判斷轉寄站清單中是否有一個項目指向來源伺服器。 如果有，請按一下 [編輯]，然後在 [編輯轉寄站] 視窗中刪除該項目。

4.  在 [DNS 管理員] 中，展開伺服器名稱，然後再展開 [正向對應區域]。

5.  針對每個正向對應區域，以滑鼠右鍵按一下區域，按一下 [內容]，然後按一下 [名稱伺服器] 索引標籤。

6.  在 [名稱伺服器] 方塊中按一下指向來源伺服器的項目，按一下 [移除]，然後按一下 [確定]。

7.  重複步驟 5 和 6，直到移除指向來源伺服器的所有項目為止。

8.  按一下 **[確定]** 關閉 **[內容]** 視窗。

9. 在 **[DNS 管理員]** 主控台中，展開 **[反向對應區域]**。

10. 重複步驟 6 到 9，移除所有指向來源伺服器的反向對應區域。

##  <a name="share-line-of-business-and-other-application-data-folders"></a><a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a> 共用企業營運和其他應用程式資料檔案夾
 您必須對複製到目的地伺服器的企業營運應用程式與其他應用程式資料資料夾，設定共用資料夾權限和 NTFS 權限。 設定許可權之後，共用資料夾會顯示在 [ **儲存體** ] 區段的 [Windows Server Essentials 儀表板] 中。

 若使用登入指令碼將磁碟機對應至共用資料夾，您必須更新指令碼為對應至目的地伺服器上的磁碟機。

##  <a name="fix-client-computer-issues-after-migrating"></a><a name="BKMK_FixClientComputerIssuesAfterMigrating"></a> 修正遷移後的用戶端電腦問題
 如果您是從 Windows Small Business Server 2003 Premium Edition 遷移至 Windows Server Essentials，並已安裝 Microsoft Internet Security and 加速 (ISA) Server，網路上的用戶端電腦仍會有 Microsoft 防火牆用戶端，並 Internet Explorer 設定為使用 proxy 伺服器。

 這會導致用戶端電腦的連線問題，因為 Proxy 伺服器已不存在。 如果未設定其他 Proxy 伺服器，用戶端電腦會繼續使用執行 Windows SBS 2003 的伺服器做為 Proxy 伺服器。 為修正此問題，您必須將 Internet Explorer 重新設定為不使用 Proxy 伺服器或使用新的 Proxy 伺服器。

#### <a name="to-reconfigure-internet-explorer"></a>重新設定 Internet Explorer

1.  在 Internet Explorer 中，按一下 [工具]，然後按一下 [網際網路選項]。

2.  依序按一下 [連線] 索引標籤、[區域網路設定]，然後執行下列其中一項：

    -   如果您的網路上未使用 Proxy 伺服器，請清除 [區域網路 (LAN) 設定] 對話方塊中的核取方塊。

    -   如果您想在網路上使用新的 Proxy 伺服器：

        1.  在 [區域網路 (LAN) 設定] 對話方塊中，清除 [自動設定] 區段中的核取方塊。

        2.  在 [Proxy 伺服器] 區段中，確認兩個核取方塊都已選取。

        3.  在 [位址] 方塊中，輸入 Proxy 伺服器的完整網域名稱 (FQDN)。

        4.  在 [連接埠] 方塊中，輸入 **80**。

3.  按兩次 **[確定]**。

4.  瀏覽至網站以確保連線設定正確。

##  <a name="give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a><a name="BKMK_AdminGroup"></a> 提供內建的 Administrators 群組以批次工作登入的許可權
 將現有的 Windows Small Business Server 2003 網域遷移到 Windows Server Essentials 之後，您應該提供內建的 Administrators 群組以批次工作登入的許可權。 確認內建的 Administrators 群組仍具有以批次工作登入目的地伺服器的權限。 系統管理員需要此權限，才能在未登入的情況下於目的地伺服器上管理警示。

#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>提供內建的 Administrators 群組以批次工作登入的權限

1. 在目的地伺服器上，開啟 [群組原則管理] 系統管理工具。

2. 在 **群組原則管理** 主控台樹中，依序展開 [**樹系：** ]、[樹系 *<\>*：]、[網域]，然後展開您的伺服器。

3. 展開 [網域控制站，以滑鼠右鍵按一下 [預設網域控制站原則]，然後按一下 [編輯]。

4. 在 **群組原則管理編輯器** 中，按一下 [**預設網域控制站原則**]<em><[伺服器名稱 \></em>**原則**]，然後展開 [**電腦** 設定]。

5. 展開 [原則]、[Windows 設定]，然後展開 [安全性設定]。

6. 在 [安全性設定] 樹狀目錄，展開 [本機原則]，然後按一下 [使用者權限指派]。

7. 在 [結果] 窗格中，以滑鼠右鍵按一下 [以批次工作登入]，然後按一下 [內容]。

8. 在 [以批次工作內容登入] 頁面，按一下 [新增使用者或群組]。

9. 在 [新增使用者或群組] 對話方塊中，按一下 [瀏覽]。

10. 在 [選取使用者、電腦或群組] 對話方塊中，輸入 **Administrators**。

11. 按一下 [檢查名稱] 以確認顯示內建的 Administrators 群組，然後按三次 [確定] 以儲存設定。

## <a name="see-also"></a>請參閱


-   [從 Windows SBS 2003 移轉](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)

-   [移轉伺服器資料到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

