---
title: AD 樹系復原移除通用類別目錄
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adds
ms.openlocfilehash: d730ce65fc179aee6a98f7cfc1a5b693bfcd6c93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817069"
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>AD 樹系復原-移除通用類別目錄  

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

 您可以使用下列程序，從 DC 移除通用類別目錄。 
  
 從備份還原的通用類別目錄伺服器，可能會導致通用類別目錄保留的其中一個部分的複本較新的資料比對應的網域，則該部分的複本。 在此情況下，較新的資料不會移除通用類別目錄，並可能甚至會複寫至其他通用類別目錄伺服器。 如此一來，即使您未還原的 DC 是通用類別目錄伺服器，可能是不小心，或是因為，單獨備份信任，您應該先移除通用類別目錄，很快就在還原作業完成之後。 移除通用類別目錄時，電腦就會移除其所有部分的複本。 
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>若要移除使用 Active Directory 站台和服務的通用類別目錄  
 
1. 開啟 伺服器管理員中，按一下**工具**然後按一下**Active Directory 站台及服務**。 
2. 在主控台樹狀目錄中，依序展開**站台**容器，然後選取適當的站台，其中包含目標伺服器。 
3. 依序展開**伺服器**容器，然後展開*伺服器*您要從中移除通用類別目錄的 dc 物件。 
4. 以滑鼠右鍵按一下**NTDS 設定**，然後按一下**屬性**。 
5. 清除**通用類別目錄**核取方塊。 
   ![移除 GC](media/AD-Forest-Recovery-Remove-GC/removegc1.png)
6. 按一下 **[套用]**。
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>若要移除通用類別目錄使用 Repadmin  
  
開啟提升權限的命令提示字元中，輸入下列命令，然後按 ENTER:  

   ```
   repadmin.exe /options DC_NAME –IS_GC  
   ```  

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)
