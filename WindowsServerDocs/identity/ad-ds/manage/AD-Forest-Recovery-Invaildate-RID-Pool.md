---
title: AD 樹系復原-失效的 RID 集區
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adds
ms.openlocfilehash: 46115991e48da301a8a739009bac27415ebe73df
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842509"
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>AD 樹系復原-讓目前的 RID 集區  

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

要使其失效的網域控制站上目前的 RID 集區使用下列程序，對我們的 Windows PowerShell。 在 Windows Server 2012 和 Windows Server 2008 R2，但未安裝 Windows Server 2008，它必須是使用預設會啟用 Windows PowerShell**將功能加入**。 它可以是[下載](https://www.microsoft.com/download/details.aspx?id=20020)在 Windows Server 2003 上執行。  

若要確認命令已順利完成，請檢查事件識別碼 16654 （來源為目錄-服務-SAM） 在 Windows Server 2012 中的事件檢視器系統記錄中。 舊版 Windows 不會記錄此事件。  
  
> [!NOTE]
> 失效的 RID 集區之後，您會收到錯誤，當您第一次嘗試建立安全性主體 （使用者、 電腦或群組）。 嘗試建立的物件就會觸發新的 RID 集區的要求。 重試此作業成功，因為將會配置新的 RID 集區。  
  
## <a name="to-invalidate-the-current-rid-pool"></a>若要使目前的 RID 集區無效  
  
- 開啟提升權限的 Windows PowerShell 工作階段中，執行下列命令，然後按 ENTER:  

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
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)
