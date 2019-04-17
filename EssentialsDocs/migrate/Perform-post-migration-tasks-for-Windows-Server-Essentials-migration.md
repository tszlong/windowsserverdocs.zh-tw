---
title: "針對 Windows Server Essentials migration1 執行後的移轉工作"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 535a547ded55cb4afc0942259eadf5222a815274
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>針對 Windows Server Essentials migration1 執行後的移轉工作

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

下列工作可協助您完成您的目的地伺服器來源伺服器上的相同設定的部分。 您可能已停用的一些這些設定來源伺服器上移轉過程中，讓它們未目的地伺服器移轉。 或是選擇性的設定步驟，您可能想要執行。  
  

-   [Delete DNS 來源伺服器的項目](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [分享的業務和其他應用程式資料資料夾](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [修正 client 後移轉的電腦問題](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [讓系統管理員建群組分批身分登入的權限](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

-   [Delete DNS 來源伺服器的項目](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [分享的業務和其他應用程式資料資料夾](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [修正 client 後移轉的電腦問題](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [讓系統管理員建群組分批身分登入的權限](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

  
##  <a name="BKMK_DeleteDNSEntries"></a>Delete DNS 來源伺服器的項目  
 您可以解除來源伺服器之後的網域名稱服務 」 (DNS) 伺服器可能仍然會包含來源伺服器指向項目。 Delete 這些 DNS 項目。  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>若要 delete 指向來源伺服器 DNS 項目  
  
1.  在目的伺服器，請打開**[DNS 管理員]**。  
  
2.  在 DNS 管理員中，以滑鼠右鍵按一下伺服器名稱，請按一下**屬性**，然後按一下 [**轉送程式**索引標籤。  
  
3.  判斷轉寄清單所指向來源伺服器中是否有一個項目。 如果有，請按一下**編輯**，然後 delete 中的項目和**編輯轉送程式**] 視窗。  
  
4.  在**DNS 管理員]**，依序展開伺服器名稱，和**正向對應區域**。  
  
5.  針對每個正向對應區域，以滑鼠右鍵按一下區域中，按一下**屬性**，，然後按一下 [**名稱伺服器]**索引標籤。  
  
6.  按一下中的項目**名稱伺服器**方塊中，按一下 [來源伺服器，指向**移除**，然後按一下 [ **[確定]**。  
  
7.  重複步驟 5 和 6 直到移除所有指標來源伺服器。  
  
8.  按一下**[確定]**以關閉 [**屬性**] 視窗。  
  
9. 在**DNS 管理員]**主控台中，展開 [**反向對應區域**。  
  
10. 重複執行步驟 6 至 9 移除所有反向對應區域，按一下 [來源伺服器。  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>分享的業務和其他應用程式資料資料夾  
 您必須設定的共用的資料夾權限和商務行和複製到目的伺服器其他應用程式資料資料夾的 NTFS 權限。 您設定的權限之後的共用的資料夾會顯示在 Windows Server Essentials 儀表板**存放裝置**一節。  
  
 如果您正在使用的登入指令碼的共用資料夾地圖磁碟機，您必須更新對應至的磁碟機目的伺服器上的指令碼。  
  
##  <a name="BKMK_FixClientComputerIssuesAfterMigrating"></a>修正 client 後移轉的電腦問題  
 如果您從 Windows 小型企業 Server 2003 Premium 版本 Microsoft 網際網路安全性和安裝加速 (ISA) 伺服器移轉到 Windows Server Essentials，仍然可以 client 電腦在網路上的 Microsoft 防火牆 Client 和 Internet Explorer 設定成使用的 proxy 伺服器。  
  
 這會造成連接的問題 client 在的電腦，因為已不存在 proxy 伺服器。 如果有不同的 proxy 伺服器設定，client 電腦繼續使用執行 Windows SBS 2003 的 proxy 伺服器的伺服器。 若要修正這個問題，您必須重新未使用的 proxy 伺服器，或使用新的 proxy 伺服器的 Internet Explorer 的設定。  
  
#### <a name="to-reconfigure-internet-explorer"></a>若要重新 Internet Explorer 設定  
  
1.  在 Internet Explorer 中，按一下 [**工具**，然後按**網際網路選項]**。  
  
2.  按一下**連接**索引標籤上，按一下 [**區域網路設定]**，然後執行下列其中一個動作：  
  
    -   如果您未使用的 proxy 伺服器您網路上，清除 [中的所有核取方塊**（區域網路）區域網路設定]**對話方塊。  
  
    -   如果您想要使用新的 proxy 伺服器，您網路上：  
  
        1.  在**（區域網路）區域網路設定]**對話方塊中，清除核取方塊，在**自動設定**一節。  
  
        2.  在**Proxy 伺服器**區段中，確認已選取這兩個核取方塊。  
  
        3.  在**位址**方塊中，輸入 proxy 伺服器的完整的網域名稱 (FQDN)。  
  
        4.  在**連接埠**方塊中，輸入**80**。  
  
3.  按一下**[確定]**兩次。  
  
4.  瀏覽的網站，以確保連接設定正確。  
  
##  <a name="BKMK_AdminGroup"></a>讓系統管理員建群組分批身分登入的權限  
 您將現有的網域 Windows 小型企業 Server 2003 移轉到 Windows Server Essentials 之後，您應該讓建系統管理員群組分批身分登入的權限。 請確認建系統管理員群組仍然目的地伺服器分批身分登入的權限。 系統管理員必須執行目的伺服器上的通知，而不登入此權限。  
  
#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>系統管理員讓建群組分批身分登入的權限  
  
1.  在目的伺服器，請打開**群組原則管理**系統管理工具。  
  
2.  在**群組原則管理**主機樹，展開 [**的樹系：***< ServerName\ >*，依序展開網域和您的伺服器。  
  
3.  展開**網域控制站**，以滑鼠右鍵按一下**預設網域控制站原則**，然後按一下 [**編輯**。  
  
4.  在**群組原則編輯器] 管理**，按一下 [**預設網域控制站原則***< ServerName\ >***原則**，然後展開**的電腦設定**。  
  
5.  展開**原則**，展開 [ **Windows 設定**，然後展開 [**的安全性設定**。  
  
6.  在**安全性設定**樹狀，展開 [**本機原則**，然後按一下 [**使用者權限指派**。  
  
7.  在 [結果] 窗格中，以滑鼠右鍵按一下**以分批登入**，然後按一下 [屬性。  
  
8.  在**以分批屬性登入**頁面上，按一下 [**新增使用者或群組**。  
  
9. 在**[新增使用者或群組**對話方塊中，按一下 [**瀏覽]**。  
  
10. 在**選擇使用者、電腦或群組**對話方塊中，輸入**系統管理員**。  
  
11. 按一下**檢查名稱]**來確認建系統管理員群組出現時，然後按**[確定]**三次，以節省設定。  
  
## <a name="see-also"></a>也了  
  

-   [從 Windows SBS 2003 移轉](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [伺服器資料移轉到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [從 Windows SBS 2003 移轉](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [伺服器資料移轉到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

