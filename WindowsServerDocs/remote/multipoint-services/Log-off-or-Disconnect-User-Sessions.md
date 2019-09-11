---
title: 登出使用者工作階段或中斷其連線
description: 瞭解如何以手動方式登出使用者
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3e9bbcdc-e33b-481e-8b46-787a4f6d58bc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 0e516a617341ffebadbdeb571a39f50369446f11
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871652"
---
# <a name="log-off-or-disconnect-user-sessions"></a>登出使用者工作階段或中斷其連線
MultiPoint 服務使用者可以像使用任何 Windows 會話一樣登入和登出其桌面會話。 使用者也可以中斷連線或暫停其會話，讓 MultiPoint 服務站不在使用中，但其會話會在 MultiPoint 服務系統的電腦記憶體中保持作用中狀態。  
  
此外，如果使用者已偏離其 MultiPoint 服務會話，或忘記登出系統，則系統管理使用者可以結束使用者的會話。  
  
## <a name="logging-off-or-disconnecting-a-session"></a>登出工作階段或將其中斷連線  
下表說明您或任何使用者可用來登出、暫停或結束工作階段的不同選項。  
  
|||  
|-|-|  
|**動作**|**Effect**|  
|按一下 [**開始**]，按一下 [設定]，按一下使用者名稱（右上角），然後按一下 [**登出**]。|工作階段隨即結束，而且站台現在可供任何使用者登入。|  
|依序按一下 [開始]、[設定]、[電源] 及 [中斷連線]。|您的工作階段隨即中斷連線，並會保留在電腦記憶體中。 站台現在可供相同使用者或不同使用者登入。|  
|按一下 [**開始**]，按一下 [設定]，按一下使用者名稱（右上角），然後按一下 [**鎖定**]。|站台隨即鎖定，而且您的工作階段會保留在電腦記憶體中。|  
  
## <a name="suspending-or-ending-a-users-session"></a>暫停或結束使用者的會話  
下表描述您身為系統管理使用者的不同選項，可以用來中斷連線或結束使用者的會話。  
  
|||  
|-|-|  
|**動作**|**Effect**|  
|**暫**在 [MultiPoint 管理員] 中，使用 [**工作站**] 索引標籤來暫停使用者的會話。 如需詳細資訊，請參閱[暫停使用者工作階段並保持使用中](Suspend-and-Leave-User-Session-Active.md)主題。|使用者的會話會結束，並保留在電腦記憶體中。 站台現在可供相同使用者或不同使用者登入。 使用者可以登入相同的站台或其他站台，並繼續其工作。|  
|**成品**在 [MultiPoint 管理員] 中，使用 [**電臺**] 索引標籤來結束使用者的會話。 您也可以在 [站台] 索引標籤上，結束所有使用者工作階段。如需詳細資訊，請參閱[結束使用者工作階段](End-a-User-Session.md)主題。|使用者的會話會結束，而且工作站會變成可供任何使用者登入。 使用者的會話不會再顯示在 [**工作站**] 索引標籤上，也不會出現在電腦記憶體中。|  
  
## <a name="see-also"></a>另請參閱  
[暫停使用者會話並保持作用中狀態](Suspend-and-Leave-User-Session-Active.md)  
[結束使用者工作階段](End-a-User-Session.md)  
[管理使用者桌面](manage-user-desktops-using-multipoint-dashboard.md)  
[登出使用者工作階段](Log-Off-User-Sessions.md)    