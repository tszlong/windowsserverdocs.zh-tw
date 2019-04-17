---
title: "廣告樹系修復未授權還原"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.technology: identity-adfs
ms.openlocfilehash: 684672491a0574b12e9b117a8e3c9c1f0936f357
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>執行 Active Directory Domain Services 未授權還原 

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2
 
 若要執行未授權的還原，請完成下列程序。  
  
 下列程序使用 Wbadmin.exe 還原未授權的 Active Directory 或 Active Directory Domain Services (AD DS)。 如果您使用不同的備份方案，或如果您想要完成的 SYSVOL 授權還原稍後的樹系的修復程序，您可以執行 SYSVOL 授權使用這些替代方法：  
  
-   如果您正在使用的檔案複製服務 (FRS) 複製 SYSVOL，請依照下列中的步驟執行[文章 290762](https://go.microsoft.com/fwlink/?LinkId=148443) Microsoft 知識庫中使用**BurFlags**初始化 FRS 複本組，或必要文章 315457 登錄鍵[315457](https://support.microsoft.com/kb/315457)重新建立 SYSVOL 樹。 若要判斷 FRS 會複寫 SYSVOL，請查看[判斷是否網域控制站的 SYSVOL 資料夾 DFSR 或 FRS 複製](https://msdn.microsoft.com/en-us/library/windows/desktop/cc507518.aspx#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs)。  
  
-   如果您正在使用散發檔案系統 (DFS) 複寫複寫 SYSVOL，請查看[執行授權同步處理 DFSR 複寫 SYSVOL 的](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。  
  
 
## <a name="performing-a-nonauthoritative-restore"></a>執行未授權還原  
 使用下列程序使用 wbadmin.exe 在執行 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 的網域控制站 AD ds 未授權的還原，且 SYSVOL 授權還原同時執行。 備份必須明確包含系統狀態的資料;適用於完整伺服器復原 server 的完整備份將會無法運作。 如需有關建立系統狀態備份，請查看[「 資料備份系統狀態](AD-Forest-Recovery-Backing-up-System-State.md)。  
  
### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>為執行 AD ds 未授權的還原和授權使用 wbadmin.exe SYSVOL  
  
-   包含**-authsysvol**切換在您的修復命令以下的範例所示：  
  
    ```  
    wbadmin start systemstaterecovery <otheroptions> -authsysvol  
    ```  
  
     例如：  
  
    ```  
    wbadmin start systemstaterecovery -version:11/20/2012-13:00 -authsysvol  
    ```  
  
 ![還原](media/AD-Forest-Recovery-Nonauthoritative-Restore/nonauth.png)

## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)
