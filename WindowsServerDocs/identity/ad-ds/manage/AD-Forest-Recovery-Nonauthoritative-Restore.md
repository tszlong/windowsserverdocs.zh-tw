---
title: AD 樹系復原-非系統授權還原
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.openlocfilehash: 9924b7498bde45f07df9c0078ff45fce84807680
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071270"
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>執行 Active Directory Domain Services 的非系統授權還原

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

若要執行非系統授權還原，請完成下列程式。

下列程式使用 Wbadmin.exe 執行 Active Directory 或 Active Directory Domain Services (AD DS) 的非系統授權還原。 如果您使用不同的備份解決方案，或您想要在稍後於樹系復原程式中完成 SYSVOL 的授權還原，您可以使用下列替代方法來執行 SYSVOL 的授權還原：

- 如果您使用「檔案複寫服務」 (FRS) 複寫 SYSVOL，請依照 Microsoft 知識庫 [文章 290762](https://go.microsoft.com/fwlink/?LinkId=148443) 中的步驟，使用 **BurFlags** 登錄機碼來重新初始化 FRS 複本集，或在必要時，使用「315457 [315457](https://support.microsoft.com/kb/315457)」來重建 sysvol 樹狀目錄。 若要判斷是否已由 FRS 複寫 SYSVOL，請參閱 [判斷是否已由 DFSR 或 FRS 複寫網域控制站的 Sysvol 資料夾](/windows/win32/vss/backing-up-and-restoring-an-frs-replicated-sysvol-folder#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs)。
- 如果您使用分散式檔案系統 (DFS) 複寫來複寫 SYSVOL，請參閱 [執行 DFSR 複寫的 sysvol 的授權同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)處理。

## <a name="performing-a-nonauthoritative-restore"></a>執行非系統授權還原

使用下列程式，在執行 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 的 DC 上使用 wbadmin.exe，同時執行 AD DS 的非系統授權還原和 SYSVOL 的授權還原。 備份必須明確包含系統狀態資料;完整伺服器復原所使用的完整伺服器備份將無法運作。 如需有關建立系統狀態備份的詳細資訊，請參閱 [備份系統狀態資料](AD-Forest-Recovery-Backing-up-System-State.md)。

### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>使用 wbadmin.exe 執行 AD DS 的非系統授權還原和 SYSVOL 的授權還原

- 在您的 recovery 命令中包含 **-authsysvol** 參數，如下列範例所示：

   ```
   wbadmin start systemstaterecovery <otheroptions> -authsysvol
   ```

   例如：

   ```
   wbadmin start systemstaterecovery -version:11/20/2012-13:00 -authsysvol
   ```

![還原](media/AD-Forest-Recovery-Nonauthoritative-Restore/nonauth.png)

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
