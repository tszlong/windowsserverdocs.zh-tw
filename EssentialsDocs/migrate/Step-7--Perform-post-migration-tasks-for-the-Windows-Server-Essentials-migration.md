---
title: "步驟 7： 執行 Windows Server Essentials 移轉後的移轉工作"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d382e3fd-d393-4bd0-883f-db50104a969f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6a61d28f29097bcb6993a471587f4cc1ae0bcc3f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="step-7-perform-post-migration-tasks-for-the-windows-server-essentials-migration"></a>步驟 7： 執行 Windows Server Essentials 移轉後的移轉工作

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

下列工作可協助您完成您的目的地伺服器來源伺服器上的相同設定的部分。 您可能已停用的一些這些設定來源伺服器上移轉過程中，讓它們未目的地伺服器移轉。  
  
1.  [來源伺服器 delete DNS 項目](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
2.  [分享的業務和其他應用程式資料資料夾](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
##  <a name="BKMK_DeleteDNSEntries"></a>來源伺服器 delete DNS 項目  
 您可以解除來源伺服器之後的網域名稱服務 」 (DNS) 伺服器可能仍然會包含來源伺服器指向項目。 Delete 這些 DNS 項目。  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>若要 delete 指向來源伺服器 DNS 項目  
  
1.  在目的伺服器，請打開**[DNS 管理員]**。  
  
2.  在 DNS 管理員中，以滑鼠右鍵按一下伺服器名稱，請按一下**屬性**，然後按一下 [**轉送程式**索引標籤。  
  
3.  判斷轉寄清單所指向來源伺服器中是否有一個項目。 如果有，請按一下**編輯**，然後 delete 中的項目和**編輯轉送程式**] 視窗。  
  
4.  在**DNS 管理員]**，依序展開伺服器名稱，和**正向對應區域**。  
  
5.  針對每個正向對應區域，以滑鼠右鍵按一下區域中，按一下**屬性**，，然後按一下 [**名稱伺服器]**索引標籤。  
  
6.  選取的項目**名稱伺服器**文字方塊中，按一下 [來源伺服器，指向**移除**，然後按一下 [ **[確定]**。  
  
7.  重複步驟 5 和 6 直到移除所有指標來源伺服器。  
  
8.  按一下**[確定]**以關閉 [**屬性**] 視窗。  
  
9. 在**[DNS 管理員]**，展開 [**反向對應區域**。  
  
10. 重複執行步驟 6 至 9 移除所有反向對應區域，按一下 [來源伺服器。  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>分享的業務和其他應用程式資料資料夾  
 您必須設定的共用的資料夾權限和商務行和複製到目的伺服器其他應用程式資料資料夾的 NTFS 權限。 共用的資料夾您設定的權限之後，會顯示在儀表板上**存放裝置**索引標籤。  
  
 如果您正在使用的登入指令碼的共用資料夾地圖磁碟機，您必須更新對應至的磁碟機目的伺服器上的指令碼。  
  
## <a name="next-steps"></a>後續步驟  
 您已針對 Windows Server Essentials 移轉執行後的移轉作業。 立即移至[在執行 Windows Server Essentials 最佳做法分析步驟 8]](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  
  

若要檢視所有的步驟，請查看[Windows Server essentials 移轉](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

