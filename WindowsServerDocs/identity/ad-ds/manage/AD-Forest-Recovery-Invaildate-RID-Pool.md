---
title: AD 樹系復原-使 RID 集區失效
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adds
ms.openlocfilehash: 9e693f6f30fb721897eaaac89b3d146c57e0e63f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823921"
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>AD 樹系復原-使目前的 RID 集區失效  

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

請使用下列程式，讓 Windows PowerShell 在網域控制站上使目前的 RID 集區失效。 Windows PowerShell 在 Windows Server 2012 和 Windows Server 2008 R2 上預設為啟用，但不是 Windows Server 2008，必須使用 [**新增功能**] 來安裝。 您可以[下載](https://www.microsoft.com/download/details.aspx?id=20020)它以在 Windows Server 2003 上執行。  

若要確認命令已順利完成，請在 Windows Server 2012 的 [系統事件檢視器記錄檔] 中檢查事件識別碼16654（來源是目錄服務-SAM）。 舊版的 Windows 不會記錄此事件。  
  
> [!NOTE]
> 讓 RID 集區失效之後，當您第一次嘗試建立安全性主體（使用者、電腦或群組）時，將會收到錯誤。 嘗試建立物件會觸發新 RID 集區的要求。 因為將配置新的 RID 集區，所以操作的重試成功。  
  
## <a name="to-invalidate-the-current-rid-pool"></a>使目前的 RID 集區失效  
  
- 開啟提升許可權的 Windows PowerShell 會話，執行下列命令，然後按 ENTER 鍵：  

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
