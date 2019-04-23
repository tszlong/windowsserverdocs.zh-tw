---
title: AD 樹系復原-重設 krbtgt 密碼
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adds
ms.openlocfilehash: 1ac0dcb9da1d10a417c128cb8498a5d8362d9a9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883739"
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>AD 樹系復原-重設 krbtgt 密碼

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

使用下列程序重設 krbtgt 密碼的網域。 下列程序適用於可寫入網域控制站，但不是唯讀的網域控制站 (Rodc)。
  
> [!IMPORTANT]
> 如果您打算在樹系復原復原線上 Rodc 時，請勿刪除 Rodc 的 krbtgt 帳戶。 RODC 的 krbtgt 帳戶會列在格式 krbtgt_*數字*。
>
> 如果您使用自訂的密碼篩選條件 （例如 passfilt.dll) 在 DC 上時，您可能會收到錯誤時嘗試重設 krbtgt 密碼。 如需詳細資訊，包括因應措施，請參閱 Microsoft Knowledge Base[文章 2549833](https://support.microsoft.com/kb/2549833) (https://support.microsoft.com/kb/2549833)。
  
## <a name="to-reset-the-krbtgt-password"></a>若要重設 krbtgt 密碼  
  
1. 按一下 **開始**，指向**控制台**，指向**系統管理工具**，然後按一下**Active Directory 使用者和電腦**。
2. 按一下 [檢視]，然後按一下 [進階功能]。
3. 在主控台樹狀目錄中，連按兩下網域容器，然後**使用者**。
4. 在 [詳細資料] 窗格中，以滑鼠右鍵按一下**krbtgt**使用者帳戶，然後按一下**重設密碼**。
   ![重設密碼](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5. 在 **新密碼**中，輸入新密碼，請重新輸入中的密碼**確認密碼**，然後按一下  **確定**。 您所指定的密碼並不重要，因為系統將會產生強式密碼自動獨立於您所指定的密碼。
  
> [!NOTE]
> 您應該執行這項作業兩次。 Krbtgt 帳戶密碼歷程記錄會為 2，這表示它包含兩個最新的密碼。 重設密碼有效地清除任何舊的密碼歷程記錄中的兩倍，因此沒有任何方法另一個 DC 會複寫使用此 DC 使用舊密碼。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md) 
