---
title: "廣告樹系修復-krbtgt 密碼重設"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adfs
ms.openlocfilehash: 445fa18503e26d04e20a61cfe652424f78631abe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>廣告樹系修復-krbtgt 密碼重設 

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

 您可以使用下列程序來重設網域 krbtgt 密碼。 下列程序適用於寫入 Dc，但不是唯讀網域控制站 (Rodc)。  
  
> [!IMPORTANT]
>  如果您打算復原 Rodc online 樹系復原時，請勿 krbtgt 帳號 rodc。 RODC krbtgt 負責格式 krbtgt_ 中列出的*號碼*。  
>   
>  如果您使用 DC 自訂的密碼篩選器 （例如 passfilt.dll)，您可能會收到一則錯誤當您嘗試重設 krbtgt 密碼。 如需詳細資訊，包括因應措施，查看 Microsoft 知識庫[文章 2549833](https://support.microsoft.com/kb/2549833) (https://support.microsoft.com/kb/2549833)。  
  
## <a name="to-reset-the-krbtgt-password"></a>若要重設 krbtgt 密碼  
  
1.  按一下**[開始]**，指向 [ **[控制台]**，指向 [**系統管理工具]**，，然後按一下**Active Directory 使用者和電腦**。  
2.  2.  按一下**檢視**，然後按**進階功能**。  
3.  在主控台按兩下網域控制站，，然後按一下**使用者**。  
4.  在詳細資料窗格中，以滑鼠右鍵按一下**krbtgt**帳號，並再按**重設密碼**。  
![重設密碼](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5.  在**新密碼**，輸入新密碼，請重新輸入密碼**確認密碼**，然後按一下 [ **[確定]**。 因為，系統將會產生自動不受影響的密碼，您可以指定穩固密碼不重要指定的密碼。  
  
    > [!NOTE]
    >  您應該先執行此作業兩次。 密碼歷史的 krbtgt 帳號為兩個，表示它包含兩個最新的密碼。 重設密碼兩次您有效清除任何歷史從舊的密碼，，就不另一個俠將複製這個網域控制站的舊的密碼。  
 
## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md) 
  
