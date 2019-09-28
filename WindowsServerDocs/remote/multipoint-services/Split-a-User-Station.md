---
title: 分割使用者站台
description: 瞭解如何分割 MultiPoint 服務中的顯示，讓兩位使用者可以使用相同的工作站
ms.custom: na
ms.date: 07/08/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f0d1fc9c-f5ea-45bc-a8da-623c5d081cdf
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 5067df3f5902570d56ee130264c751d66b5b0d3f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394945"
---
# <a name="split-a-user-station"></a>分割使用者站台
任何大於 1024 x 768 解析度的 MultiPoint 服務站台螢幕，都可以使用 [站台] 索引標籤上的 [Split station (分割站台)] 工作來分割成兩個站台。在分割發生時，螢幕上所顯示的桌面會移至螢幕左半部，並在相同螢幕右半部建立新的站台。 新的站台必須對應到鍵盤、滑鼠及 USB 集線器以完成其建立程序。 分割站台後，使用者可以登入到左方站台，而另一個使用者則可登入右方站台。  
  
使用分割螢幕工作站的優點可能包括：  
  
-   藉由在 MultiPoint 服務系統上容納更多的學生以降低成本和空間  
  
-   允許兩個學生在專案上並存共同作業  
  
-   允許老師在學生遵循其他站台時向其示範站台上的程序  
   
> [!NOTE]  
> 當您分割站台時，會暫停站台上使用中的工作階段。 使用者必須在進行分割之後再次登入站台，以繼續執行工作。  
  
**若要分割站：**  
  
1.  在 [MultiPoint 管理員] 的 [站模式] 中，按一下 [**工作站**] 索引標籤。  
  
2.  在 [站台] 欄位中，按一下您想要分割的站台名稱。  
  
3.  在 [Stations Tasks (站台工作)] 中，按一下 [Split station (分割站台)]。  
  
**若要將分割站交還給單一工作站：**  
  
1.  在 [MultiPoint 管理員] 的 [站模式] 中，按一下 [**工作站**] 索引標籤。  
  
2.  在 [站台] 欄位中，按一下您想要結束分割的站台名稱。  
  
3.  在 [Stations Tasks (站台工作)] 中，按一下 [Unsplit station (復原分割站台)]。  
  
## <a name="see-also"></a>另請參閱  
[管理使用者站台](Manage-User-Stations.md)