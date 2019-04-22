---
title: 暫停使用者工作階段並保持使用中
description: 了解如何暫停 MultiPoint 的工作階段的使用者，而不中斷連線
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
ms.openlocfilehash: cc4310e6f7609464cf037b750bec6e5e805e0b26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815219"
---
# <a name="suspend-and-leave-user-session-active"></a>暫停使用者工作階段並保持使用中
您可以中斷連線，或不想結束使用者工作階段時，暫停從 MultiPoint 服務系統的使用者。 使用者也可以自行中斷工作階段連線，而不是由您為他們中斷工作階段連線。 當使用者工作階段被暫停時，此工作階段仍會在 MultiPoint 服務系統的電腦記憶體中保持使用中狀態，直到電腦關機或重新啟動為止。 此時，所有暫停的工作階段會結束，並將遺失所有未儲存的工作。  
  
1.  在站台模式中開啟 MultiPoint 管理員，然後按一下**站台** 索引標籤。  
  
2.  在 [電腦] 欄位中，按一下您要暫停其工作階段之電腦的名稱。  
  
3.  在 [Stations Tasks (站台工作)] 下，按一下 [Suspend all stations (暫停所有站台)]。  
  
使用者工作階段暫停之後，使用者可以登入相同的站台或其他站台，然後繼續在原本的工作階段中工作。  
  
## <a name="see-also"></a>另請參閱  
[管理使用者桌面](manage-user-desktops-using-multipoint-dashboard.md)  
[登出或中斷使用者工作階段](Log-off-or-Disconnect-User-Sessions.md)