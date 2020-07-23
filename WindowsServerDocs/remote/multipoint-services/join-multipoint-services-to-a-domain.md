---
title: 將 MultiPoint 服務加入網域（選擇性）
Description: 提供將 MultiPoint 服務加入網域的步驟
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 623b7c21-dcbb-402e-8b5a-8e434cd225bd
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: caa7611b47bd856648415358137609be41d4fca0
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959110"
---
# <a name="join-the-multipoint-services-computer-to-a-domain-optional"></a>將 MultiPoint 服務電腦加入網域（選擇性）
如果您將透過 Active Directory 網域存取 MultiPoint 服務電腦，下一步就是將電腦新增至網域。  
  
> [!IMPORTANT]  
> 將電腦加入網域之前，您必須先驗證您的時區。 如需指示，請參閱[設定日期、時間和時區](Set-the-date--time--and-time-zone.md)。  
   
1.  從 [開始]**** 畫面，開啟 [控制台]****。 按一下 [**系統及安全性**]，然後按一下 [**系統**]。  
  
2.  在 [電腦名稱、網域及工作群組設定]**** 底下，按一下 [變更設定]****。  
  
3.  在 [**電腦名稱稱**] 索引標籤上，按一下 [**變更**]。  
  
4.  在 [**電腦名稱稱/網域變更**] 對話方塊中，選取 [**網域**]，輸入網域的名稱，按一下 **[確定**]，然後遵循嚮導中的步驟來完成程式。  
  
5.  在電腦重新開機之後，以系統管理員身分登入，並等候 MultiPoint 管理員開啟。  
  
> [!IMPORTANT]  
> 為確保 MultiPoint 服務網域部署正常運作，您必須設定幾個群組原則，並更新登錄。 如需相關資訊，請參閱[設定網域部署的群組原則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265982(v=ws.11))。  
