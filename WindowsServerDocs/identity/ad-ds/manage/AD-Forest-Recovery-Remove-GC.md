---
title: "廣告樹系修復移除通用"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adfs
ms.openlocfilehash: b7da7c03cbae4691e8f902f1be0cb33c0c912248
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>廣告樹系修復-移除通用  

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

 若要移除 DC 通用使用下列程序。  
  
 從備份還原通用伺服器可能會導致通用按住其部分複本的對應的部分複本授權網域比較新的資料。 在這些案例中，較新的資料將不會從通用，甚至可能會複寫其他通用伺服器。 如此一來，即使您還原是一個通用伺服器，可能不小心俠或孤獨備份，這是因為您信任的您應該會移除通用即將還原作業完成之後。 當移除通用時，電腦將會移除所有部分的複本。  
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>若要移除通用使用 Active Directory 網站和服務  
 
1.  打開伺服器管理員中，按一下**工具**，按一下 [ **Active Directory 網站和服務**。  
2.  在主控台中，展開**網站**容器、，然後選取包含目標伺服器的適當網站。  
3.  展開**伺服器]**容器，然後展開*伺服器*針對您要移除的通用網域控制站物件。  
4.  以滑鼠右鍵按一下**NTDS 設定**，然後按**屬性**。  
5.  清除**通用**核取方塊。  
![移除 GC](media/AD-Forest-Recovery-Remove-GC/removegc1.png)
6.  按一下**適用於**。
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>若要移除通用使用 Repadmin  
  
1.  打開提升權限的命令提示字元中，輸入下列命令，並按下 ENTER:  
  
    ```  
    repadmin.exe /options DC_NAME –IS_GC  
    ```  
  
 ## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)
