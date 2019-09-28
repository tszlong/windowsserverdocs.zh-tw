---
title: 硬體需求和效能建議
description: 提供 MultiPoint 服務的硬體和效能需求和建議
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99a5c9c2-270f-4753-a28c-434882c03125
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 284131028b308ee86389f25102d934390ba2f16d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389109"
---
# <a name="hardware-requirements-and-performance-recommendations"></a>硬體需求和效能建議
本主題描述執行 MultiPoint 服務系統所需的硬體，並支援使用者應用程式案例。 使用者案例會直接影響 CPU、RAM 和網路頻寬需求。  

## <a name="optimize-multipoint-services-system-performance"></a>將 MultiPoint 服務系統效能優化  
MultiPoint 服務系統的效能會受到 CPU、GPU 和執行 MultiPoint 服務的電腦上可用的 RAM 數量的功能直接影響。  
  
### <a name="applications-and-internet-content"></a>應用程式和網際網路內容  
由於 MultiPoint 服務是共用資源運算解決方案，因此在工作站上執行的應用程式類型和數目可能會影響 MultiPoint 服務系統的效能。 請務必考慮您在規劃系統時，定期使用的程式類型。 例如，需要大量圖形的應用程式所需的電腦比像是文字處理器的應用程式更強大。 使用需要大量圖形的應用程式來多載電腦，可能會造成整個系統的延遲問題。  
  
應用程式所存取的內容類型也會影響系統的效能。 如果有多個工作站使用網頁瀏覽器來存取多媒體內容（例如，全動作影片），則可以連接較少的工作站，然後才會對系統效能造成負面影響。 相反地，如果多個工作站使用網頁瀏覽器來存取靜態 web 內容，則可以連接更多的工作站，而不會對效能產生重大影響。  
  
### <a name="hardware-recommendations"></a>硬體建議  
若要在各種負載下達到 MultiPoint 服務系統的良好效能，請在規劃和測試系統時，使用下表中的指導方針。 這些是 forMultiPoint 服務的基本需求。 實際的設定大小取決於您的系統組態、您正在執行的工作負載，以及硬體功能。 您應該一律藉由測試您的應用程式和硬體來進行驗證。  
  
> [!NOTE]  
> 2C = 2 核心，4C = 4 核心，6C = 6 核心，MT = 多執行緒。 處理器速度至少應為 2.0 ghz。  
  
### <a name="minimum-recommended-hardware-for-running-default-multipoint-server-stations"></a>執行預設 MultiPoint 伺服器工作站的最低建議硬體  
  
|應用程式案例|最多5個工作站|6-8 電臺|9-12 電臺|13-16 電臺|17-20 電臺|21-24 電臺|  
|------------------------|----------------------|-------------------|------------------|-------------------|-------------------|-----------------|  
|**生產力**<br /><br />Office、web 流覽、企業營運應用程式|CPU：上文<br /><br />RAM：2 GB|CPU：上文<br /><br />RAM：4 GB|CPU：4C<br /><br />RAM：6 GB|CPU：4C<br /><br />RAM：8 GB|CPU：4C + MT 或6C<br /><br />RAM：10 GB| CPU：6C + MT<br /><br />RAM：12 GB|
|**型**<br /><br />Office、網頁流覽、企業營運應用程式，以及某些使用者偶爾使用的影片|CPU：上文<br /><br />RAM：2 GB|CPU：上文<br /><br />RAM：4 GB|CPU：4C<br /><br />RAM：6 GB|CPU：4C + MT 或6C<br /><br />RAM：8 GB|CPU：6C + MT<br /><br />RAM：10 GB| CPU：6C + MT<br /><br />RAM：12 GB| 
|**需要大量影片**<br /><br />Office、web 流覽、企業營運應用程式，以及所有使用者經常使用的影片，**請注意：** 影片測試是使用原生解析度的 360p h.264 影片來執行。|CPU：4C + MT<br /><br />RAM：2 GB|CPU：6C + MT<br /><br />RAM：4 GB|CPU：8C + MT<br /><br />RAM：6 GB|CPU：12C + MT<br /><br />RAM：8 GB|CPU：16C + MT<br /><br />RAM：10 GB<br /><br />-瘦用戶端：RemoteFX<br />-不建議使用 USB 影片| CPU：20C + MT<br /><br />RAM：12 GB<br /><br />-瘦用戶端：RemoteFX<br />-不建議使用 USB 影片|   
  
## <a name="minimum-recommended-hardware-for-running-full-windows-10-virtual-desktops"></a>執行完整 Windows 10 虛擬桌面的最低建議硬體  
針對每個工作站執行完整的虛擬作業系統實例，比執行預設的 MultiPoint 桌面會話更需要大量運算資源，因此每部工作站的主機硬體需求也會更高：  
  
1.  CPU：每站1個核心或執行緒  
  
2.  固態硬碟（SSD）  
  
    1.  容量 > = 每一站 20 gb + 40GB （WMS 主機作業系統）  
  
    2.  隨機讀取/寫入 IOPS > = 每台工作站3K  
  
3.  RAM > = 每一站 2GB GB + 2GB （適用于 WMS 主機作業系統）  
  
BIOS CPU 設定已設定為啟用虛擬化-第二層位址轉譯（SLAT）  
  
如需有關為您的需求選擇最佳 MultiPoint 服務硬體的詳細資訊，請洽詢您的硬體廠商。  