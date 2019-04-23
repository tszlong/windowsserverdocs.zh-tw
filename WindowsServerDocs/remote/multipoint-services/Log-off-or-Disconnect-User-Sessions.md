---
title: 登出使用者工作階段或中斷其連線
description: 了解如何以手動方式將使用者登出
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
ms.openlocfilehash: 518e9dc9ba9603d988a7e21e08caa29db9f04bde
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854939"
---
# <a name="log-off-or-disconnect-user-sessions"></a>登出使用者工作階段或中斷其連線
MultiPoint 服務使用者可以登入並登出他們的桌面工作階段，如同處理任何 Windows 工作階段。 使用者也可以中斷連線，或暫停其工作階段，如此可在 MultiPoint 服務站未使用，但其工作階段保留在 MultiPoint 服務系統的電腦記憶體中。  
  
此外，系統管理使用者可以結束使用者工作階段，如果使用者已離開其 MultiPoint 服務工作階段或忘記登出系統。  
  
## <a name="logging-off-or-disconnecting-a-session"></a>登出工作階段或將其中斷連線  
下表說明您或任何使用者可用來登出、暫停或結束工作階段的不同選項。  
  
|||  
|-|-|  
|**動作**|**Effect**|  
|按一下 **開始**，按一下 設定 中，按一下 使用者名稱 （右上角），然後按一下**登出**。|工作階段隨即結束，而且站台現在可供任何使用者登入。|  
|依序按一下 [開始]、[設定]、[電源] 及 [中斷連線]。|您的工作階段隨即中斷連線，並會保留在電腦記憶體中。 站台現在可供相同使用者或不同使用者登入。|  
|按一下 **開始**，按一下 設定 中，按一下 使用者名稱 （右上角），然後按一下**鎖定**|站台隨即鎖定，而且您的工作階段會保留在電腦記憶體中。|  
  
## <a name="suspending-or-ending-a-users-session"></a>暫停或結束使用者的工作階段  
下表說明身為系統管理使用者的您可用來將使用者的工作階段中斷連線或結束的不同選項。  
  
|||  
|-|-|  
|**動作**|**Effect**|  
|**暫停：** 在 MultiPoint 管理員 中，使用**站台**暫停使用者工作階段 索引標籤。 如需詳細資訊，請參閱[暫停使用者工作階段並保持使用中](Suspend-and-Leave-User-Session-Active.md)主題。|使用者的工作階段隨即結束，並會保留在電腦記憶體中。 站台現在可供相同使用者或不同使用者登入。 使用者可以登入相同的站台或其他站台，並繼續其工作。|  
|**結束：** 在 MultiPoint 管理員 中，使用**站台**索引標籤來結束使用者工作階段。 您也可以在 [站台] 索引標籤上，結束所有使用者工作階段。如需詳細資訊，請參閱[結束使用者工作階段](End-a-User-Session.md)主題。|使用者的工作階段隨即結束，而且站台現在可供任何使用者登入。 使用者的工作階段不會再顯示於 [站台] 索引標籤，而且也不會在電腦記憶體中。|  
  
## <a name="see-also"></a>另請參閱  
[暫停使用者工作階段並保持使用中](Suspend-and-Leave-User-Session-Active.md)  
[結束使用者工作階段](End-a-User-Session.md)  
[管理使用者桌面](manage-user-desktops-using-multipoint-dashboard.md)  
[登出使用者工作階段](Log-Off-User-Sessions.md)    