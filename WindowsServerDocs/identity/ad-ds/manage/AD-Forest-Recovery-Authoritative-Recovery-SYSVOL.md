---
title: AD 樹系復原-SYSVOL 的授權同步
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.openlocfilehash: 8d4df4b7b893c0c6e8fc6b467ee115e9013335f9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956895"
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>AD 樹系復原-執行 DFSR 複寫 SYSVOL 的授權同步處理

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

有不同的方式可執行 SYSVOL 的授權還原。 您可以編輯**MsDFSR 選項**屬性，或使用 wbadmin – authsysvol 來執行系統狀態還原。 如果您可以選擇還原系統狀態備份 (也就是將 AD DS 還原到相同的硬體和作業系統實例) 然後使用 wbadmin – authsysvol 比較簡單。 但是，如果您需要執行裸機還原，則需要編輯**MsDFSR 選項**屬性。

使用下列步驟來執行 SYSVOL (的授權同步處理（如果它是使用 DFSR) 藉由編輯 MsDFSR 屬性來複寫 **）** 。 如果使用 FRS 複寫 SYSVOL，請參閱[文章 290762](https://go.microsoft.com/fwlink/?LinkId=148443)。

## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>執行 DFSR 複寫 SYSVOL 的權威同步處理

1. 開啟 [Active Directory 使用者和電腦]。
2. 按一下 [ **View**]，然後選取 [**使用者]、[連絡人]、[群組] 和 [電腦] 作為容器**和 [ **Advanced Features**]

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png)

3. 在樹狀檢視中，依序按一下 [**網域控制站**]、您還原的 DC 名稱、[ **DFSR-LocalSettings**] 和 [**網域系統磁碟區**]。

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)

4. 在詳細資料窗格中，以滑鼠右鍵按一下 [ **SYSVOL 訂閱**]，按一下 [**屬性**]，然後按一下 [**屬性編輯器**]。

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png)

5. 按一下 [ **msDFSR-選項**]，按一下 [**編輯**]，輸入**1**，然後按一下 **[確定]** 。

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png)

6. 按一下 **[確定]** 以關閉 [屬性編輯器]。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
