---
title: "廣告樹系修復-失效 RID 集區"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adfs
ms.openlocfilehash: cb024356ae5f872e93448d73ea54b271fe3fae4d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>廣告樹系修復-失效目前 RID 集區  

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

 使用下列程序給我們的 Windows PowerShell 失效網域控制站目前 RID 集區。 Windows PowerShell 在 Windows Server 2012 和 Windows Server 2008 R2，但不是 Windows Server 2008，就必須安裝使用預設支援**[新增功能**。 它可以是[下載](https://www.microsoft.com/download/details.aspx?id=20020)在 Windows Server 2003 上執行。  
  
 若要確認命令已成功完成，檢查 263 16654 （來源是 Directory-服務-坡） 在 Windows Server 2012 中事件檢視器中系統登入。 較舊的 Windows 版本不登入這個事件。  
  
> [!NOTE]
>  您使 RID 集區之後，您會收到一則錯誤，當您第一次建立安全性原則 （使用者、 電腦或群組） 時。 嘗試將建立物件觸發 RID 新集區的要求。 重試作業的成功因為將配置 RID 新集區。  
  
## <a name="to-invalidate-the-current-rid-pool"></a>若要使目前移除集區  
  
1.  打開提升權限的 Windows PowerShell 工作階段，執行下列命令並按下 ENTER 鍵：  
  
    ```  
    $Domain = New-Object System.DirectoryServices.DirectoryEntry  
    $DomainSid = $Domain.objectSid  
    $RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")  
    $RootDSE.UsePropertyCache = $false  
    $RootDSE.Put("invalidateRidPool", $DomainSid.Value)  
    $RootDSE.SetInfo()  
    ```  
  
## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)
