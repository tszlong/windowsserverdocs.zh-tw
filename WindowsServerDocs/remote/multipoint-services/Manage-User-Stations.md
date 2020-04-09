---
title: 管理使用者站台
description: 瞭解如何管理 MultiPoint 服務中的使用者工作站
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: b418578d-3a4c-49b0-90db-8389b320b2f6
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 7b434002b5f542e3a9242290217fa66d418ee2f1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853491"
---
# <a name="manage-user-stations"></a>管理使用者站台
本節討論如何管理組成 MultiPoint 服務系統的「站台」。 管理 MultiPoint 服務系統同時包含管理 MultiPoint 管理員的硬體和軟體元件。 在 MultiPoint 服務系統中，桌面是在每部使用者工作站的監視器上顯示的軟體使用者介面。  
  
## <a name="station-status"></a>站台狀態  
您可以在 [**工作站**] 索引標籤上，針對每個桌面視圖下列狀態類型。狀態包括：  
  
-   已登入的使用者  
  
-   已暫停但在電腦上仍為使用中的使用者工作階段  
  
-   所使用的站台及使用者  
  
如需檢視桌面狀態的詳細資訊，請參閱[檢視使用者連線狀態](View-User-Connection-Status.md)主題。  

>[!TIP] 
> 您可以為每個站台指派易記名稱，以便更輕鬆地識別站台。 使用 [Identify station (識別站台)]，即可在指派的螢幕上顯示站台名稱。
  
## <a name="different-ways-to-log-standard-users-off-of-the-multipoint-services-system"></a>將標準使用者登出 MultiPoint 服務系統的不同方式  
身為「系統管理使用者」，您可以隨時登出 Windows，而不會影響 MultiPoint 服務系統中的作用中使用者。 「標準使用者」也可以將其工作階段「中斷連線」或「登出」MultiPoint 服務系統。 啟用磁碟保護時，如果使用者當天要離開，則應該確保將其工作儲存在電腦或外接式存放裝置上；如此一來，即使關閉 MultiPoint 服務系統，使用者改天仍可擷取其儲存的工作。  
  
身為系統管理使用者，您可能需要結束標準使用者的*會話*，而不是讓使用者登出。 您可以使用下列兩種方式的其中一種來結束標準使用者的會話：  
  
-   結束工作階段並將使用者登出。 如需結束使用者會話的詳細資訊，請參閱[結束使用者會話](End-a-User-Session.md)主題。  
  
-   暫止使用者以暫時結束使用者的會話，但在 MultiPoint 服務系統的電腦記憶體中讓會話保持作用中狀態。 已暫停的使用者可以從同一個站台或不同站台重新連線到工作階段以繼續工作。 如需暫停使用者會話的詳細資訊，請參閱[暫停使用者會話並保持](Suspend-and-Leave-User-Session-Active.md)使用中主題。  
  
## <a name="set-a-station-to-automatically-log-on"></a>設定站台以自動登入  
身為系統管理使用者，您可以設定在執行 MultiPoint 服務的電腦啟動時要自動登入一或多個站台。 如需自動登入的詳細資訊，請參閱[設定站台以自動登入](Set-up-a-Station-for-Automatic-Logon.md) 主題。  
  
## <a name="split-a-station"></a>分割站台  
如果任何站台監視器的解析度大於 1024x768，則可以分割成兩個站台。 如需分割到站台的詳細資訊，請參閱[分割使用者站台](Split-a-User-Station.md)主題。  
  
## <a name="see-also"></a>另請參閱  
[檢視使用者連線狀態](View-User-Connection-Status.md)  
[登出使用者工作階段或中斷其連線](Log-off-or-Disconnect-User-Sessions.md)  
[暫停使用者會話並保持作用中狀態](Suspend-and-Leave-User-Session-Active.md)  
[設定工作站以自動登入](Set-up-a-Station-for-Automatic-Logon.md)  
[結束使用者工作階段](End-a-User-Session.md)  
[分割使用者站台](Split-a-User-Station.md)