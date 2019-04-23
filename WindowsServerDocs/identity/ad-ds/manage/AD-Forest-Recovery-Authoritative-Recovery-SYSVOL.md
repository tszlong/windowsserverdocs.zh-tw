---
title: AD 樹系復原 SYSVOL 的權威的同步處理
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adds
ms.openlocfilehash: 246a2ea589ee05110362cff99d50e93a0a18c2b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826999"
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>AD 樹系復原-執行權威的同步處理的 DFSR 複寫的 SYSVOL  

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

有不同的方式執行 SYSVOL 的系統授權還原。 您可以編輯**msDFSR 選項**屬性或執行使用 wbadmin – authsysvol 系統狀態還原。 如果您有還原系統狀態備份選項 （也就您要還原 AD DS 相同的硬體和作業系統執行個體） 則使用 wbadmin – authsysvol 是更簡單。 但如果您需要執行裸機還原，則您必須編輯**msDFSR 選項**屬性。  

使用下列步驟執行權威的同步處理的 SYSVOL （如果它使用 DFSR 複寫），藉由編輯**msDFSR 選項**屬性。 如果使用 FRS 來複寫 SYSVOL，請參閱[文章 290762](https://go.microsoft.com/fwlink/?LinkId=148443)。  

## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>若要執行的 DFSR 複寫 SYSVOL 的權威同步處理  

1. 開啟 [Active Directory 使用者和電腦]。  
2. 按一下 **檢視**，然後選取**使用者、 連絡人、 群組和電腦做為容器**並**進階功能**。 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 

3. 在樹狀檢視中，按一下**網域控制站**，在還原時，DC 的名稱**DFSR LocalSettings**，然後**Domain System Volume**。 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  

4. 在 [詳細資料] 窗格中，以滑鼠右鍵按一下**SYSVOL 訂用帳戶**，按一下**屬性**，然後按一下**屬性編輯器**。  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 

5. 按一下  **msDFSR 選項**，按一下**編輯**，型別**1**，然後按一下**確定**  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 

6. 按一下 **確定**以關閉 屬性編輯器。  
  
## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)
