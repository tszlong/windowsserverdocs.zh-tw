---
title: AD 樹系復原-將 RID 集區失效
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.openlocfilehash: 36919eb4f4e67129446975f9c8c4d60fd64cc499
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939638"
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>AD 樹系復原-使目前的 RID 集區失效

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

您可以使用下列程式 Windows PowerShell，讓網域控制站上的目前 RID 集區失效。 Windows PowerShell 在 Windows Server 2012 和 Windows Server 2008 R2 上預設為啟用，但 Windows Server 2008 必須使用 [ **新增功能**] 來安裝。 您可以 [下載](https://www.microsoft.com/download/details.aspx?id=20020) 該檔案以在 Windows Server 2003 上執行。

若要確認命令是否已順利完成，請檢查事件識別碼 16654 (來源是否為 Windows Server 2012 中事件檢視器之系統記錄檔中的目錄服務-SAM) 。 舊版 Windows 不會記錄此事件。

> [!NOTE]
> 將 RID 集區失效之後，當您第一次嘗試建立安全性主體 (使用者、電腦或群組) 時，您將會收到錯誤。 嘗試建立物件會觸發新 RID 集區的要求。 因為將配置新的 RID 集區，所以作業的重試成功。

## <a name="to-invalidate-the-current-rid-pool"></a>使目前的 RID 集區失效

- 開啟提升許可權的 Windows PowerShell 會話，執行下列命令，然後按 ENTER：

   ```powershell
   $Domain = New-Object System.DirectoryServices.DirectoryEntry
   $DomainSid = $Domain.objectSid
   $RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")
   $RootDSE.UsePropertyCache = $false
   $RootDSE.Put("invalidateRidPool", $DomainSid.Value)
   $RootDSE.SetInfo()
   ```

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
