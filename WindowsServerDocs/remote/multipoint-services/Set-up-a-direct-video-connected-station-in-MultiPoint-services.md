---
title: 設定在 MultiPoint 服務中直接視訊連線站台
description: 了解如何在 MultiPoint 服務中建立直接視訊連線的站台
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82ba3517-9743-4cde-8eea-63a17edb016f
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 58197164c91ab6b69b0ef331c025287f593f94c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850739"
---
# <a name="set-up-a-direct-video-connected-station-in-multipoint-services"></a>設定在 MultiPoint 服務中直接視訊連線站台
直接連接至視訊的站台上，此監視器是直接連接到 MultiPoint 伺服器電腦上的視訊連接埠。 將鍵盤和滑鼠然後連接到 USB 集線器，並與監視相關聯。  
  
下圖顯示具有單一的 MultiPoint 伺服器電腦和四個直接視訊連線的站台的 MultiPoint 伺服器環境。 如需詳細資訊，請參閱 < [MultiPoint Server 站台](MultiPoint-services-Stations.md)。  
  
**四個影片的直接連線的 multiPoint 服務系統**  
  
![MultiPoint 服務 USB 架構系統配置的影像](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
> [!NOTE]  
> 若要設定直接視訊連線的站台，您必須使用拉丁文鍵盤 （例如英文或西班牙文語言鍵盤）。  
  
## <a name="to-set-up-a-direct-video-connected-station"></a>若要設定直接的視訊連接站台  
  
1.  如下所示，請將監視器纜線連接到的電腦上，視訊顯示連接埠。  
  
    ![視訊連線到 USB 集線器架構系統的影像](./media/WMSVideoConnection.gif) 
  
2.  將視訊監視器的電源線連接到電源插座。  
  
3.  如下所示，請連接到的電腦上，開啟的 USB 連接埠的 USB 集線器。  
  
    ![MultiPoint 服務 USB 集線器連線的影像](./media/WMSUSBHubConnection.gif)  
  
4.  將鍵盤和滑鼠接上 USB 站台集線器。  
  
    ![USB 集線器輸入裝置連線的影像](./media/WMSUSBDeviceConnection.gif)  
  
5.  連接到 USB 集線器的任何其他的週邊設備，例如耳機。  
  
6.  如果您使用外部供電的集線器，中樞的電源線接上電源插座。  
  
    > [!IMPORTANT]  
    > 我們強烈建議使用供電的集線器。 不足的目前條件可能不穩定的系統行為。  
    >   
    > 使用者應該不滑鼠及鍵盤直接附加到電腦的 USB 連接埠。 這樣做很可能完全造成不正確的多個鍵盤和滑鼠相同的站台，或沒有站台關聯。  
  
7.  依照出現在監視器 來建立站台的指示。  
  
如果您將多個直接的視訊連接站台加入您的 MultiPoint 服務環境時，可能會變更的主要站台。 您可以輕鬆地找出其直接視訊連線的工作站是您的主要站台。  
  
## <a name="to-find-out-which-direct-video-connected-station-is-the-primary-station"></a>若要了解其直接視訊連線的站台是主要站台  
  
1.  開啟直接連線到電腦的顯示卡 （視訊卡） 的所有監視。  
  
2.  啟動 （或重新啟動） 的 MultiPoint 服務電腦和哪些監視器 」 會顯示啟動畫面，請參閱。 站台是主要站台。  
  
    > [!NOTE]  
    > 在某些情況下，BIOS 啟動資訊會顯示在多個監視器同時。 在此情況下，任何監視可用於存取 BIOS 視為 「 主要站台 」。