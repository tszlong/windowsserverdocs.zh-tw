---
title: "廣告樹系修復 SYSVOL 的授權同步"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adfs
ms.openlocfilehash: 5bdc619ddf9f28fb074e90dfbf22a7be076a05d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>廣告樹系修復-執行 DFSR 複寫 SYSVOL 授權同步處理  

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

 有不同的方式執行 SYSVOL 授權。 您可以編輯**msDFSR 選項**屬性或執行使用 wbadmin – authsysvol 系統狀態還原。 如果您有要還原備份系統狀態的選項（也就您要還原 AD DS 相同的硬體和作業系統執行個體）就會使用 wbadmin – authsysvol 簡單。 但如果您需要執行枯金屬還原，則您要編輯**msDFSR 選項**屬性。  
  
 使用下列步驟來編輯執行 SYSVOL 授權同步處理（如果這複製使用 DFSR）**msDFSR 選項**屬性。 如果使用 FRS 複製 SYSVOL，查看[文章 290762](https://go.microsoft.com/fwlink/?LinkId=148443)。  
  
## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>若要執行的 DFSR 複寫 SYSVOL 授權同步處理  
  
1.  打開 Active Directory 使用者與電腦。  
2.  按一下**檢視**，，然後選取 [**使用者、連絡人、群組和電腦容器為**並**進階功能**。 
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 
3.  在樹檢視中，按一下 [**網域控制站**，DC 還原您的名稱**DFSR-LocalSettings**，然後**網域系統磁碟區**。 
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  
4.  在詳細資料窗格中，以滑鼠右鍵按一下**SYSVOL 裝機費**，按一下 [**屬性**，並按**屬性編輯器**。  
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 
5.  按一下**msDFSR 選項**，按一下**編輯**，輸入**1**，然後按一下**[確定]**  
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 
6.  按一下**[確定]**以關閉屬性編輯器。  
  
## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)
