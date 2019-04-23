---
title: AD 樹系復原-非系統授權還原
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.technology: identity-adds
ms.openlocfilehash: eae4cab2bd709097fe0efd0745baeb0ec685abc7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829609"
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>執行非系統授權還原的 Active Directory 網域服務 

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

若要執行非系統授權還原，請完成下列程序。  
  
下列程序會使用 Wbadmin.exe 執行 Active Directory 或 Active Directory 網域服務 (AD DS) 的非系統授權還原。 如果您使用不同的備份解決方案，或如果您想要完成 SYSVOL 的授權還原，稍後在樹系修復程序，您可以使用這些替代的方法來執行系統授權還原的 SYSVOL:  
  
- 如果您使用檔案複寫服務 (FRS) 來複寫 SYSVOL，請依照下列中的步驟[文章 290762](https://go.microsoft.com/fwlink/?LinkId=148443) Microsoft 知識庫中使用**BurFlags**登錄機碼以重新初始化 FRS 複本設定此項目，或如有必要，文章 315457 [315457](https://support.microsoft.com/kb/315457)重建 SYSVOL 樹狀目錄。 若要判斷是否 FRS 來複寫 SYSVOL，請參閱[DFSR 或 FRS 來複寫判斷是否網域控制站的 SYSVOL 資料夾](https://msdn.microsoft.com/en-us/library/windows/desktop/cc507518.aspx#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs)。  
- 如果您使用分散式檔案系統 (DFS) 複寫來複寫 SYSVOL，請參閱[執行權威的同步處理的 DFSR 複寫的 SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。  

## <a name="performing-a-nonauthoritative-restore"></a>執行非系統授權還原

使用下列程序來執行 AD DS 的非系統授權還原和 SYSVOL 的授權還原，同時使用 wbadmin.exe 在執行 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 網域控制站。 備份必須明確包含系統狀態資料;使用於完整伺服器復原完整伺服器備份將無法運作。 如需建立系統狀態備份的詳細資訊，請參閱 <<c0> [ 備份系統狀態資料](AD-Forest-Recovery-Backing-up-System-State.md)。  
  
### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>若要執行非系統授權還原 AD ds 和權威還原使用 wbadmin.exe 的 SYSVOL  
  
- 包含 **-authsysvol**切換在修復命令中，在下列範例所示：  

   ```  
   wbadmin start systemstaterecovery <otheroptions> -authsysvol  
   ```  

   例如:   

   ```  
   wbadmin start systemstaterecovery -version:11/20/2012-13:00 -authsysvol  
   ```  
  
![還原](media/AD-Forest-Recovery-Nonauthoritative-Restore/nonauth.png)

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)
