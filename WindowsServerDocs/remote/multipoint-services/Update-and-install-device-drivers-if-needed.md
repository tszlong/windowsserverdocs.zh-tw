---
title: 視需要更新及安裝裝置驅動程式
description: 了解如何檢查並更新裝置驅動程式在 MultiPoint 服務
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16be3ef9-a05b-4621-a431-5806b567e997
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 66477634e06df217656876b084ae37be8cb311c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829129"
---
# <a name="update-and-install-device-drivers-if-needed"></a>視需要更新及安裝裝置驅動程式
如果您使用 usb 極簡型用戶端或需要的驅動程式的週邊設備，您應該在這一次安裝驅動程式。 它是個不錯的主意，也要檢查**裝置管理員**任何驅動程式的警示，和安裝這些裝置的驅動程式。  
  
一般而言，最新的驅動程式所需的下列類型的裝置：  
  
-   Usb 極簡型用戶端  
  
-   USB over Ethernet 零用戶端  
  
-   磁碟控制卡  
  
-   網路介面卡  
  
-   音效控制器  
  
-   USB 主控制器

-   圖形卡


## <a name="to-check-for-driver-alerts-in-device-manager"></a>若要檢查驅動程式在裝置管理員中的警示  
  
1.  開啟 [開始] 畫面。  
  
2.  型別**電腦管理**，然後按一下**電腦管理**結果中。  
  
3.  在 [電腦管理] 主控台樹狀目錄中，按一下**裝置管理員**。  
  
4.  在系統上的裝置的權限，請檢查驅動程式可能會影響 MultiPoint Server 的警示。  
  
## <a name="to-install-device-drivers-in-multipoint-manager"></a>在 MultiPoint 管理員安裝裝置驅動程式  
  
1.  若要開啟 MultiPoint 管理員，請搜尋 「 MultiPoint 管理員 」，然後按一下**MultiPoint 管理員**結果中。  
  
2.  在 MultiPoint 管理員] 中，按一下**首頁**索引標籤，然後再按一下**切換至 [主控台模式**。  
  
3.  若要安裝裝置驅動程式，按兩下 驅動程式檔案，並遵循指示來安裝驅動程式。  
  
4.  重複上述步驟以安裝所有必要的驅動程式。  
  
    > [!NOTE]  
    > 如果安裝需要重新啟動電腦，您必須切換到主控台模式，然後再安裝下一步 的驅動程式。 MultiPoint server 永遠會在站台模式中啟動。 若要切換到主控台模式，請移至**首頁**MultiPoint 管理員中索引標籤，然後按一下**切換至 主控台模式**。