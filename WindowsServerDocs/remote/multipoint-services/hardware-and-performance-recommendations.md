---
title: 硬體需求和效能建議
description: 提供的 MultiPoint 服務的硬體和效能需求和建議
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99a5c9c2-270f-4753-a28c-434882c03125
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b47f4c224d4a4f6f4aa104b6d5ee5d93590ac0c4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823389"
---
# <a name="hardware-requirements-and-performance-recommendations"></a>硬體需求和效能建議
本主題描述，才能執行 MultiPoint 服務系統，並支援使用者應用程式案例的硬體。 使用者案例會直接影響 CPU、 RAM 和網路頻寬需求。  

## <a name="optimize-multipoint-services-system-performance"></a>將 MultiPoint 服務系統效能最佳化  
MultiPoint 服務系統的效能將會直接受到 CPU、 GPU 和是執行 MultiPoint 服務電腦上可用的 RAM 數量的功能。  
  
### <a name="applications-and-internet-content"></a>應用程式和網際網路內容  
因為 MultiPoint 服務會共用的資源的運算解決方案，在站台執行的應用程式的數目與類型可能會影響 MultiPoint 服務系統的效能。 請務必考慮定期在您計劃您的系統時所使用的程式類型。 比方說，使用大量圖形的應用程式需要的應用程式，例如使用文書處理器更強大的電腦。 多載具有大量圖形的應用程式的電腦可能會導致在整個系統的延遲問題。  
  
存取應用程式的內容類型也會影響系統的效能。 如果多個站台使用網頁瀏覽器來存取多媒體內容，例如完整的動態視訊，較少的站台可以連接之前造成負面影響系統效能。 相反地，如果多個站台使用網頁瀏覽器來存取靜態 web 內容，更多的站台可以連接而不需要對效能產生重大影響。  
  
### <a name="hardware-recommendations"></a>硬體建議  
若要達到良好的效能和您 MultiPoint 服務系統在各種負載下，當您規劃和測試您的系統下, 表中使用的指導方針。 這些是基本需求 forMultiPoint 服務。 實際的設定調整大小取決於您的系統設定、 您正在執行，工作負載和硬體功能。 您應該一律驗證測試您的應用程式和硬體。  
  
> [!NOTE]  
> 2 C = 2 核心，4 C = 4 核心，6 C = 6 個核心，MT = 多執行緒處理。 處理器速度應該至少 2.0 ghz （含）。  
  
### <a name="minimum-recommended-hardware-for-running-default-multipoint-server-stations"></a>建議的硬體執行預設 MultiPoint Server 站台的最小值  
  
|應用程式案例|最多 5 個站台|6-8 站台|9 至 12 站台|13-16 站台|17-20 個站台|21-24 站台|  
|------------------------|----------------------|-------------------|------------------|-------------------|-------------------|-----------------|  
|**產能**<br /><br />Office、 web 瀏覽、 特定業務應用程式|CPU：2C<br /><br />RAM：2 GB|CPU：2C<br /><br />RAM：4 GB|CPU：4C<br /><br />RAM：6 GB|CPU：4C<br /><br />RAM：8 GB|CPU：C + MT 4 或 6 C<br /><br />RAM：10 GB| CPU：6C+MT<br /><br />RAM：12 GB|
|**混合**<br /><br />Office、 web 瀏覽、 特定業務應用程式和偶爾視訊部分使用者使用|CPU：2C<br /><br />RAM：2 GB|CPU：2C<br /><br />RAM：4 GB|CPU：4C<br /><br />RAM：6 GB|CPU：C + MT 4 或 6 C<br /><br />RAM：8 GB|CPU：6C+MT<br /><br />RAM：10 GB| CPU：6C+MT<br /><br />RAM：12 GB| 
|**需要大量的影片**<br /><br />Office、 web 瀏覽、 特定業務應用程式和頻繁影片供所有使用者**附註：** 設定測試的影片，被執行使用原生解析度的 360p H.264 視訊。|CPU：4C+MT<br /><br />RAM：2 GB|CPU：6C+MT<br /><br />RAM：4 GB|CPU：8C+MT<br /><br />RAM：6 GB|CPU：12C+MT<br /><br />RAM：8 GB|CPU：16C+MT<br /><br />RAM：10 GB<br /><br />-精簡型用戶端：RemoteFX<br />不建議使用的 USB 影片| CPU：20 的 C + MT<br /><br />RAM：12 GB<br /><br />-精簡型用戶端：RemoteFX<br />不建議使用的 USB 影片|   
  
## <a name="minimum-recommended-hardware-for-running-full-windows-10-virtual-desktops"></a>最小的建議執行完整的 Windows 10 虛擬桌上型電腦的硬體  
執行完整的虛擬作業系統執行個體的每個站台是更多計算比執行預設 MultiPoint 桌面工作階段，因此每個站台的主機硬體需求高的大量資源：  
  
1.  CPU：1 個核心或每個站台的執行緒  
  
2.  固態硬碟 (SSD)  
  
    1.  容量 > = 針對單一站台的 20 GB + 40 GB，如 「 WMS 主機作業系統  
  
    2.  隨機讀取/寫入 IOPS > = 3 K 針對單一站台  
  
3.  RAM > = 針對單一站台的 2 GB + 2GB WMS 裝載作業系統  
  
若要啟用虛擬化-第二層位址轉譯 (SLAT) 已設定 BIOS CPU 設定  
  
如需有關如何選擇最佳的 MultiPoint 服務硬體，針對您的需求的詳細資訊，請連絡您的硬體廠商。  