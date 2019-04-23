---
title: 使用視訊裝置
description: 了解如何監視影片和投影機使用 MultiPoint 服務中的站台
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f7f5a97-efd2-4184-8ad3-cf029d615eab
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: d828ea911aaff27a1df79d0380dfe92987c3d2aa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844199"
---
# <a name="work-with-video-devices"></a>使用視訊裝置
了解視訊裝置 (例如監視器或投影機) 連接到 MultiPoint 服務系統中的電腦或 MultiPoint 服務「站台」時的運作方式。  
  
## <a name="working-with-video-monitors"></a>使用視訊監視器  
根據您的 MultiPoint 服務系統硬體，有兩種連接視訊監視器的方式︰  
  
-   針對*USB 集線器架構系統*，視訊監視器纜線連接到的電腦上，開啟的視訊連接埠，如下圖所示：  
  
    ![視訊連線到 USB 集線器架構系統的影像](./media/WMSVideoConnection.gif)  
  
-   針對*多功能集線器架構的系統*使用內建視訊支援，將視訊監視器纜線連接到多功能集線器上的視訊連接埠：  
  
    ![多功能集線器與視訊連線的影像](./media/WMSMultifunctionHubVideoConnection.gif)  
  
如需詳細資訊，請參閱[設定站台](Set-Up-a-Station.md)主題。  
  
## <a name="working-with-video-projectors"></a>使用視訊投影機  
當您想要投影大型影像以供他人檢視時 (例如在實驗室中)，可以將視訊投影機連接到您的 MultiPoint 服務系統。 這兩個 USB 集線器架構及和多功能集線器架構站台，您有兩個選項，可將投影機連接到站台：  
  
-   以投影機取代監視器，然後使用投影機作為該站台的顯示裝置，如下圖所示：  
  
    ![連接到電腦的投影機影像](./media/WMSVideoProjectorConnection.gif)  
  
-   取得視訊分接裝置，以將投影機和監視器同時連接到站台的視訊連接埠。  
  
    MultiPoint 服務會在兩個顯示裝置上顯示相同的影像。 不想要投影時，您可以關閉投影機，只使用視訊監視器。  
  
使用任一選項時，請注意下列事項︰  
  
-   連接視訊顯示器時，您可能需要再次「與站台建立關聯」，MultiPoint 服務才能正確識別新的顯示器。 遵循站台之視訊顯示裝置上所顯示的指示。  
  
-   您可能需要取得轉接頭或轉換裝置，才能在 DVI 與 VGA 插頭之間轉換。  
  
-   使用 "Y" 型分接纜線可能會降低兩個視訊裝置上的視訊品質。  
  
-   透過 "Y" 型分接纜線同時使用投影機和監視器時，MultiPoint 服務會將兩個裝置的螢幕解析度調整為兩個裝置中最大解析度較低者的解析度，通常是投影機的解析度。  
  
-   MultiPoint 服務不支援將單一站台的顯示器延伸橫跨多部監視器。  
  
## <a name="see-also"></a>另請參閱  
[管理站台硬體](Manage-Station-Hardware.md)  
[設定站台](Set-Up-a-Station.md) 