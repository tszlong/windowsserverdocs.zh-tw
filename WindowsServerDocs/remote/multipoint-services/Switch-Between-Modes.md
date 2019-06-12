---
title: 在模式間切換
description: 了解如何在 MultiPoint 服務中的站台和主控台模式之間切換
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f1b2324-c1b0-4b61-ab51-39af15e7792a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: a9c00d65ad1c59e91f4011ab932bf9f921957c59
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446154"
---
# <a name="switch-between-modes"></a>在模式間切換
MultiPoint 管理員包含可協助您執行不同類型的 MultiPoint 服務系統管理模式如下：  
  
-   *站台模式*:根據預設，MultiPoint 服務系統會在站台模式中啟動。 處於站台模式時，MultiPoint 服務站台會將每個站台當作執行 Windows 的獨立電腦，而多位使用者可同時使用該系統。 您與您的使用者可共用檔案及執行必要工作。  
  
-   *主控台模式*:當 MultiPoint 服務系統處於主控台模式時，您可以安裝和更新的軟體和驅動程式或執行其他維護工作。 當系統處於主控台模式時，其他電腦使用者無法使用任何*站台*。 這類站台不會顯示在 MultiPoint 管理員。 直接連線到伺服器的所有監視會被都視為此電腦系統的顯示。   
  
> [!NOTE]
> 您可以變更伺服器設定中的預設，藉此強制系統以主控台模式啟動。  
> ## <a name="to-switch-from-station-mode-to-console-mode"></a>從站台模式切換到主控台模式  
  
1.  在站台模式中開啟 MultiPoint 管理員，然後按一下**首頁** 索引標籤。  
  
2.  在 [電腦]  欄位中，按一下您要變更其模式的電腦。  
  
3.  底下*電腦名稱***工作**，按一下 **切換到主控台模式**。 電腦會重新啟動，而所有站台變成無法使用。  
  
## <a name="to-switch-from-console-mode-to-station-mode"></a>從主控台模式切換到站台模式  
  
1.  在主控台模式中，開啟 MultiPoint 管理員，然後按一下**首頁** 索引標籤。  
  
2.  在 [電腦]  欄位中，按一下您要變更其模式的電腦。  
  
3.  底下*電腦名稱***工作**，按一下 **切換至 站台模式**。 電腦會重新啟動，而所有站台變成可供使用。  
  
## <a name="see-also"></a>另請參閱  
[使用 MultiPoint 管理員管理系統工作](Manage-System-Tasks-Using-MultiPoint-Manager.md)