---
title: 使用視訊裝置
description: 瞭解視頻監視器與投影機如何使用 MultiPoint 服務中的工作站
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b7019000c99295204f196ee918129cded02e084f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389255"
---
# <a name="work-with-video-devices"></a>使用視訊裝置
了解視訊裝置 (例如監視器或投影機) 連接到 MultiPoint 服務系統中的電腦或 MultiPoint 服務「站台」時的運作方式。  
  
## <a name="working-with-video-monitors"></a>使用視訊監視器  
根據您的 MultiPoint 服務系統硬體，有兩種連接視訊監視器的方式︰  
  
-   若是以*USB 集線器為基礎的系統*，請將視頻監視器纜線連接到電腦上開啟的影片埠，如下圖所示：  
  
    ![視訊連線到 USB 集線器架構系統的影像](./media/WMSVideoConnection.gif)  
  
-   針對具有內建影片支援的*多功能中樞型系統*，請將視頻監視器纜線連接到多功能集線器上的影片埠：  
  
    ![多功能集線器與視訊連線的影像](./media/WMSMultifunctionHubVideoConnection.gif)  
  
如需詳細資訊，請參閱[設定站台](Set-Up-a-Station.md)主題。  
  
## <a name="working-with-video-projectors"></a>使用視訊投影機  
當您想要投影大型影像以供他人檢視時 (例如在實驗室中)，可以將視訊投影機連接到您的 MultiPoint 服務系統。 對於 USB 集線器和以多功能集線器為基礎的工作站，您有兩個選項可將投影機連接到工作站：  
  
-   以投影機取代監視器，然後使用投影機作為該站台的顯示裝置，如下圖所示：  
  
    ![連接到電腦的投影機影像](./media/WMSVideoProjectorConnection.gif)  
  
-   取得影片分隔器裝置，將投影機和監視器連接到工作站的視訊連接埠。  
  
    MultiPoint 服務會在兩個顯示裝置上顯示相同的影像。 不想要投影時，您可以關閉投影機，只使用視訊監視器。  
  
使用任一選項時，請注意下列事項︰  
  
-   連接視訊顯示器時，您可能需要再次「與站台建立關聯」，MultiPoint 服務才能正確識別新的顯示器。 請遵循工作站的影片顯示裝置上顯示的指示。  
  
-   您可能需要取得轉接頭或轉換裝置，才能在 DVI 與 VGA 插頭之間轉換。  
  
-   使用 "Y" 型分接纜線可能會降低兩個視訊裝置上的視訊品質。  
  
-   透過 "Y" 型分接纜線同時使用投影機和監視器時，MultiPoint 服務會將兩個裝置的螢幕解析度調整為兩個裝置中最大解析度較低者的解析度，通常是投影機的解析度。  
  
-   MultiPoint 服務不支援跨多個監視器擴充單一工作站的顯示。  
  
## <a name="see-also"></a>另請參閱  
[管理站台硬體](Manage-Station-Hardware.md)  
[設定站台](Set-Up-a-Station.md) 