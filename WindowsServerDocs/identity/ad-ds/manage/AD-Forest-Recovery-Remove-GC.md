---
title: AD 樹系復原-移除通用類別目錄
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.openlocfilehash: b05415e73faef73831cccbbd9785dd1cf2d1cf9e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969845"
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>AD 樹系復原-移除通用類別目錄

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 使用下列程式從 DC 移除通用類別目錄。

 從備份還原通用類別目錄伺服器可能會導致通用類別目錄保留其中一個部分複本的較新資料，而非該部分複本授權的對應網域。 在這種情況下，將不會從通用類別目錄中移除較新的資料，甚至可能會複寫到其他通用類別目錄伺服器。 如此一來，即使您還原的是通用類別目錄伺服器的 DC，可能不小心或因為這是您信任的單獨備份，因此您應該在還原作業完成之後，立即移除通用類別目錄。 移除通用類別目錄時，電腦會移除其所有的部分複本。

## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>若要使用 Active Directory 的網站和服務移除通用類別目錄

1. 開啟伺服器管理員，按一下 [**工具**]，然後按一下 [ **Active Directory 網站和服務**]。
2. 在主控台樹中，展開 [ **Sites** ] 容器，然後選取包含目標伺服器的適當網站。
3. 展開 [**伺服器**] 容器，然後展開您要移除其通用類別目錄之 DC 的*伺服器*物件。
4. 以滑鼠右鍵按一下 [ **NTDS 設定**]，然後按一下 [**屬性**]。
5. 清除 [**通用類別目錄**] 核取方塊。
   ![移除 GC](media/AD-Forest-Recovery-Remove-GC/removegc1.png)
6. 按一下 [套用]。

## <a name="to-remove-the-global-catalog-using-repadmin"></a>使用 Repadmin 移除通用類別目錄

開啟提升許可權的命令提示字元，輸入下列命令，然後按 ENTER 鍵：

   ```
   repadmin.exe /options DC_NAME –IS_GC
   ```

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
