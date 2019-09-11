---
title: 暫停使用者工作階段並保持使用中
description: 瞭解如何在不中斷連線的情況下，從 MultiPoint 會話暫停使用者
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5263bce3-fe92-4398-8393-2e3a4e05d530
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: a7c94b9d1edd36efc8651e35dfabbc95239335cb
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871526"
---
# <a name="suspend-and-leave-user-session-active"></a>暫停使用者工作階段並保持使用中
當您不想要結束使用者的會話時，可以中斷連線或暫停 MultiPoint 服務系統的使用者。 使用者也可以自行中斷工作階段連線，而不是由您為他們中斷工作階段連線。 當使用者會話暫停時，會話會在 MultiPoint 服務系統的電腦記憶體中保持作用中狀態，直到電腦關機或重新開機為止。 此時，所有暫停的工作階段會結束，並將遺失所有未儲存的工作。  
  
1.  開啟 [站模式] 中的 [MultiPoint 管理員]，然後按一下 [**工作站**] 索引標籤。  
  
2.  在 [電腦] 欄位中，按一下您要暫停其工作階段之電腦的名稱。  
  
3.  在 [Stations Tasks (站台工作)] 下，按一下 [Suspend all stations (暫停所有站台)]。  
  
使用者工作階段暫停之後，使用者可以登入相同的站台或其他站台，然後繼續在原本的工作階段中工作。  
  
## <a name="see-also"></a>另請參閱  
[管理使用者桌面](manage-user-desktops-using-multipoint-dashboard.md)  
[登出使用者工作階段或中斷其連線](Log-off-or-Disconnect-User-Sessions.md)