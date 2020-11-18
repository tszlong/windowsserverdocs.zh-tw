---
title: AD 樹系復原-重設 krbtgt 密碼
description: 如何重設網域的 krbtgt 密碼。
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.openlocfilehash: 0fc21aa9daf6ed528d5501f1703c847c03a8aff7
ms.sourcegitcommit: 7f859d8ec86664fdedd05901ac3714f84e7868b5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94703764"
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>AD 樹系復原-重設 krbtgt 密碼

> 適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 及 2008 R2

使用下列程式來重設網域的 krbtgt 密碼。 下列程式會將可寫入的 Dc （但不是唯讀網域控制站） (Rodc) 。

> [!IMPORTANT]
> 如果您計畫在樹系復原期間線上復原 Rodc，請勿刪除 Rodc 的 krbtgt 帳戶。 RODC 的 krbtgt 帳戶列示 krbtgt_ *號碼* 格式。
>
> 如果您使用自訂的密碼篩選器 (例如 DC 上的 passfilt.dll) ，則當您嘗試重設 krbtgt 密碼時，可能會收到錯誤。 如需詳細資訊（包括解決方法），請參閱 Microsoft 知識庫 [文章 2549833](https://support.microsoft.com/kb/2549833) (https://support.microsoft.com/kb/2549833) 。

## <a name="to-reset-the-krbtgt-password"></a>重設 krbtgt 密碼

1. 按一下 [ **開始**]，指向 [ **主控台**]，指向 [系統 **管理工具**]，然後按一下 [ **Active Directory 消費者和電腦**]。

2. 按一下 [檢視]，然後按一下 [進階功能]。

3. 在主控台樹中，按兩下 [網域] 容器，然後按一下 [ **使用者**]。

4. 在詳細資料窗格中，以滑鼠右鍵按一下 **krbtgt** 使用者帳戶，然後按一下 [ **重設密碼**]。

   ![重設密碼](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)

5. 在 [ **新密碼**] 中輸入新密碼，在 [ **確認密碼**] 中重新輸入密碼，然後按一下 **[確定]**。 您指定的密碼並不重要，因為系統會自動產生與您指定的密碼無關的強式密碼。

> [!IMPORTANT]
> 您應該執行此作業兩次。 重設金鑰發佈中心服務帳戶密碼兩次時，重設之間必須有10小時的等待期間。 10小時是預設的 **使用者票證存留期上限** ，以及 **服務票證原則設定的最長存留** 期，因此，在變更最大存留期期間的情況下，重設之間的最小等候期間應大於設定的值。  

> [!NOTE]
> Krbtgt 帳戶的密碼歷程記錄值為2，表示它包含2個最新的密碼。 藉由重設密碼兩次，您可以有效地清除歷程記錄中的任何舊密碼，如此一來，就無法使用舊密碼將其他 DC 複寫到此 DC。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)

- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
