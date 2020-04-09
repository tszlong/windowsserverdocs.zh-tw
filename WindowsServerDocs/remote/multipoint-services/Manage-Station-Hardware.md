---
title: 管理站台硬體
description: 提供如何管理 MultiPoint 工作站硬體的總覽
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 429b8539-b17a-4e01-9576-860600466451
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 8ffb6fd714293471a0e9aa020390943b201261c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853521"
---
# <a name="manage-station-hardware"></a>管理站台硬體
MultiPoint 服務系統是由單一電腦與至少一個「站台」所組成。 站台硬體通常是由「站台集線器」、滑鼠、鍵盤和視訊監視器所組成。 站台通常以實體方式接線到電腦。  
  
下圖顯示具有四個站台之 MultiPoint 服務系統的配置範例︰ 每個工作站都會使用 USB 集線器和多監視器視訊卡來連接到 MultiPoint 服務電腦。 此圖並未顯示使用「多功能集線器」連接的站台。  
   
![MultiPoint 服務以 USB 為基礎的系統版面配置影像](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
本節中的主題說明如何檢視連接到 MultiPoint 服務系統之硬體的狀態，並提供可用來設定 MultiPoint 服務站台之 USB 裝置和其他周邊硬體裝置類型的詳細資訊。 以下簡短說明本節中的主題，可協助您選取硬體並設定您的 MultiPoint 服務站台。  
  
## <a name="view-hardware-status"></a>檢視硬體狀態  
在 [MultiPoint 管理員] 中，您可以使用 [**工作站**] 索引標籤，來查看連線到 MultiPoint 服務電腦的工作站和硬體裝置的狀態。 如需如何檢視 MultiPoint 服務電腦及其所連接之硬體裝置狀態的詳細資訊，請參閱[檢視硬體狀態](View-Hardware-Status.md)主題。  
  
## <a name="work-with-usb-devices"></a>使用 USB 裝置  
當 USB 裝置和其他周邊硬體裝置連接到 MultiPoint 服務系統時，連接到 MultiPoint 服務電腦與連接到 MultiPoint 服務站台的運作方式不同。 [使用 USB 裝置](Work-with-USB-Devices.md)主題說明不同的硬體裝置在這些狀況下的運作方式，並提供如何使用站台集線器的詳細資訊。  
  
## <a name="work-with-video-devices"></a>使用視訊裝置  
[使用視訊裝置](Work-with-Video-Devices.md)主題討論視訊裝置 (例如監視器或投影機) 連接到 MultiPoint 服務系統中的電腦或 MultiPoint 服務站台時的運作方式。  
  
## <a name="set-up-a-station"></a>設定站台  
[設定站台](Set-Up-a-Station.md)主題說明如何將周邊硬體裝置連接到 MultiPoint 服務站台集線器，以建立 MultiPoint 服務站台。 MultiPoint 服務支援兩種站台集線器類型︰  
  
-   外部供電或匯流排驅動的多埠*USB 集線器*，可支援滑鼠、鍵盤和其他 USB 週邊裝置  
  
-   「多功能集線器」，可包含整合式視訊控制卡，以及滑鼠、鍵盤和音訊周邊裝置的連接埠。  
  
這兩種類型的站台集線器都是透過 USB 纜線連接到電腦。 [設定站台](Set-Up-a-Station.md)主題中的程序說明如何將硬體裝置連接到各種類型的站台集線器。  
  
## <a name="see-also"></a>另請參閱  
[檢視硬體狀態](View-Hardware-Status.md)  
[使用 USB 裝置](Work-with-USB-Devices.md)  
[使用視訊裝置](Work-with-Video-Devices.md)  
[設定站台](Set-Up-a-Station.md)