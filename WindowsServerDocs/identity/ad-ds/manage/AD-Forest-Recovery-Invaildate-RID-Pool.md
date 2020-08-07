---
title: AD 樹系復原-使 RID 集區失效
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.openlocfilehash: 1dac48ff5dc9f864b7c23f4a91740005fa12780c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969885"
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>AD 樹系復原-使目前的 RID 集區失效

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

請使用下列程式，讓 Windows PowerShell 在網域控制站上使目前的 RID 集區失效。 Windows PowerShell 在 Windows Server 2012 和 Windows Server 2008 R2 上預設為啟用，但不是 Windows Server 2008，必須使用 [**新增功能**] 來安裝。 您可以[下載](https://www.microsoft.com/download/details.aspx?id=20020)它以在 Windows Server 2003 上執行。

若要確認命令已順利完成，請檢查事件識別碼 16654 (來源是 Windows Server 2012 中事件檢視器系統記錄檔中的 [目錄服務-SAM) ]。 舊版的 Windows 不會記錄此事件。

> [!NOTE]
> 當您使 RID 集區失效之後，您會在第一次嘗試建立安全性主體 (使用者、電腦或群組) 時收到錯誤。 嘗試建立物件會觸發新 RID 集區的要求。 因為將配置新的 RID 集區，所以操作的重試成功。

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
