---
title: 在模式間切換
description: 瞭解如何在 MultiPoint 服務中切換站與主控台模式
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 5f1b2324-c1b0-4b61-ab51-39af15e7792a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 80b56fcf5aca3530e41fba0125341031bb8112a7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855571"
---
# <a name="switch-between-modes"></a>在模式間切換
MultiPoint 管理員包含下列模式，可協助您執行不同類型的 MultiPoint 服務系統管理：  
  
-   *站台模式*：根據預設，MultiPoint 服務系統會以站台模式啟動。 處於站台模式時，MultiPoint 服務站台會將每個站台當作執行 Windows 的獨立電腦，而多位使用者可同時使用該系統。 您與您的使用者可共用檔案及執行必要工作。  
  
-   *主控台模式*：當 MultiPoint Services 系統處於主控台模式時，您可以安裝及更新軟體與驅動程式，或執行其他維護工作。 當系統處於主控台模式時，其他電腦使用者無法使用任何*站台*。 這類工作站不會顯示在 [MultiPoint 管理員] 中。 直接連線到伺服器的所有監視器都會被視為此電腦系統的顯示。   
  
> [!NOTE]
> 您可以變更伺服器設定中的預設，藉此強制系統以主控台模式啟動。  
> ## <a name="to-switch-from-station-mode-to-console-mode"></a>從站台模式切換到主控台模式  
  
1.  以站模式開啟 MultiPoint 管理員，然後按一下 [**首頁**] 索引標籤。  
  
2.  在 [電腦] 欄位中，按一下您要變更其模式的電腦。  
  
3.  在 [*電腦名稱稱*工作]**下，按一下**[**切換到主控台模式]** 。 電腦會重新啟動，而所有站台變成無法使用。  
  
## <a name="to-switch-from-console-mode-to-station-mode"></a>從主控台模式切換到站台模式  
  
1.  以主控台模式開啟 MultiPoint 管理員，然後按一下 [**首頁**] 索引標籤。  
  
2.  在 [電腦] 欄位中，按一下您要變更其模式的電腦。  
  
3.  在 [*電腦名稱稱*工作]**下，按一下**[**切換到工作站模式]** 。 電腦會重新啟動，而所有站台變成可供使用。  
  
## <a name="see-also"></a>另請參閱  
[使用 MultiPoint 管理員管理系統工作](Manage-System-Tasks-Using-MultiPoint-Manager.md)