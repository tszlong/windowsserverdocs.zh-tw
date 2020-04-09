---
title: AD 樹系復原-重設 krbtgt 密碼
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adds
ms.openlocfilehash: e2d19621c80b40fa70706bf75434dce74693990d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823631"
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>AD 樹系復原-重設 krbtgt 密碼

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

使用下列程式重設網域的 krbtgt 密碼。 下列程式會套用可寫入的 Dc，而不是唯讀網域控制站（Rodc）。
  
> [!IMPORTANT]
> 如果您想要在樹系復原期間線上復原 Rodc，請不要刪除 Rodc 的 krbtgt 帳戶。 RODC 的 krbtgt 帳戶會以 krbtgt_*號碼*的格式列出。
>
> 如果您在 DC 上使用自訂的密碼篩選器（例如 passfilt.dll），則當您嘗試重設 krbtgt 密碼時，可能會收到錯誤。 如需詳細資訊，包括因應措施，請參閱 Microsoft 知識庫[文章 2549833](https://support.microsoft.com/kb/2549833) （ https://support.microsoft.com/kb/2549833)。
  
## <a name="to-reset-the-krbtgt-password"></a>重設 krbtgt 密碼  
  
1. 按一下 [**開始**]，指向 [**控制台**]，指向 [系統**管理工具**]，然後按一下 [ **Active Directory 使用者和電腦**]。
2. 按一下 [檢視]，然後按一下 [進階功能]。
3. 在主控台樹中，按兩下網域容器，然後按一下 [**使用者**]。
4. 在詳細資料窗格中，以滑鼠右鍵按一下**krbtgt**使用者帳戶，然後按一下 [**重設密碼**]。
   ![重設密碼](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5. 在 [**新密碼**] 中輸入新密碼，然後在 [**確認密碼**] 中重新輸入密碼，再按一下 **[確定]** 。 您指定的密碼並不重要，因為系統會自動產生與您所指定之密碼無關的強式密碼。
  
> [!NOTE]
> 您應該執行此作業兩次。 Krbtgt 帳戶的密碼歷程記錄是2，這表示它包含兩個最新的密碼。 藉由重設密碼兩次，您可以有效地清除歷程記錄中的任何舊密碼，因此不會有其他 DC 使用舊密碼與此 DC 複寫的方式。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md) 
