---
title: 步驟 7：執行 Windows Server Essentials 移轉的移轉後工作
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819159"
---
# <a name="step-7-perform-post-migration-tasks-for-the-windows-server-essentials-migration"></a>步驟 7：執行 Windows Server Essentials 移轉的移轉後工作

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

下列工作可協助您完成目的地伺服器設定，其中一些設定與來源伺服器上的設定相同。 您可能已在移轉過程中停用來源伺服器上其中某些設定，這樣它們就不會移轉到目的地伺服器。  
  
1.  [刪除來源伺服器的 DNS 項目](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
2.  [共用的業務和其他應用程式資料資料夾](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
##  <a name="BKMK_DeleteDNSEntries"></a> 刪除來源伺服器的 DNS 項目  
 在您解除委任來源伺服器之後，網域名稱服務 (DNS) 伺服器可能仍然會包含指向來源伺服器的項目。 請刪除這些 DNS 項目。  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>若要刪除指向來源伺服器的 DNS 項目  
  
1.  在目的地伺服器上，開啟 [DNS 管理員]。  
  
2.  在 [DNS 管理員] 中，以滑鼠右鍵按一下伺服器名稱，按一下 [內容] ，然後按一下 [轉寄站]  索引標籤。  
  
3.  判斷轉寄站清單中是否有一個項目指向來源伺服器。 如果有，請按一下 [編輯]，然後在 [編輯轉寄站] 視窗中刪除該項目。  
  
4.  在 [DNS 管理員] 中，展開伺服器名稱，然後再展開 [正向對應區域]。  
  
5.  針對每個正向對應區域，以滑鼠右鍵按一下區域，按一下 [內容]，然後按一下 [名稱伺服器] 索引標籤。  
  
6.  在 [名稱伺服器] 核取方塊中選取指向來源伺服器的項目，按一下 [移除]，然後按一下 [確定]。  
  
7.  重複步驟 5 和 6，直到移除指向來源伺服器的所有項目為止。  
  
8.  按一下 **[確定]** 關閉 **[內容]** 視窗。  
  
9. 在 [DNS 管理員] ，展開 [反向對應區域] 。  
  
10. 重複步驟 6 到 9，移除所有指向來源伺服器的反向對應區域。  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a> 共用的業務和其他應用程式資料資料夾  
 您必須對複製到目的地伺服器的企業營運應用程式與其他應用程式資料資料夾，設定共用資料夾權限和 NTFS 權限。 設定權限之後，共用資料夾會顯示在儀表板的 [儲存體]  索引標籤上。  
  
 若使用登入指令碼將磁碟機對應至共用資料夾，您必須更新指令碼為對應至目的地伺服器上的磁碟機。  
  
## <a name="next-steps"></a>後續步驟  
 您執行 Windows Server Essentials 移轉的移轉後工作。 現在請移至[步驟 8-執行 Windows Server Essentials Best Practices Analyzer](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  
  

若要檢視所有步驟，請參閱[移轉至 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

