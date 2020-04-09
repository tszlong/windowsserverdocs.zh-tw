---
title: 在 MultiPoint 服務中設定直接視頻連線的工作站
description: 瞭解如何在 MultiPoint 服務中建立直接視頻連線的工作站
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 82ba3517-9743-4cde-8eea-63a17edb016f
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 4bab07a21c6e2de529797e240325b3ff27446fe0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859351"
---
# <a name="set-up-a-direct-video-connected-station-in-multipoint-services"></a>在 MultiPoint 服務中設定直接視頻連線的工作站
在直接連接視頻的工作站上，監視器會直接連線到 MultiPoint Server 電腦上的視訊連接埠。 鍵盤和滑鼠接著會連接到 USB 集線器，並與監視相關聯。  
  
下圖顯示多點伺服器環境，其中包含一部 MultiPoint Server 電腦和四個直接連接視頻的工作站。 如需詳細資訊，請參閱[MultiPoint Server 電臺](MultiPoint-services-Stations.md)。  
  
**具有四個直接視頻連線的 MultiPoint 服務系統**  
  
![MultiPoint 服務以 USB 為基礎的系統版面配置影像](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
> [!NOTE]  
> 若要設定直接連接視頻的站，您必須使用拉丁鍵盤（例如英文或西班牙文語言鍵盤）。  
  
## <a name="to-set-up-a-direct-video-connected-station"></a>設定直接連接視頻的工作站  
  
1.  將監視器纜線連接到電腦上的影片顯示埠，如下所示。  
  
    ![視訊連線到 USB 集線器架構系統的影像](./media/WMSVideoConnection.gif) 
  
2.  將視訊監視器的電源線連接到電源插座。  
  
3.  將 USB 集線器連接到電腦上開啟的 USB 埠，如下所示。  
  
    ![MultiPoint 服務 USB 集線器連線的影像](./media/WMSUSBHubConnection.gif)  
  
4.  將鍵盤和滑鼠連接到 USB 站集線器。  
  
    ![USB 集線器輸入裝置連線的影像](./media/WMSUSBDeviceConnection.gif)  
  
5.  將任何其他週邊設備（例如耳機）連接到 USB 集線器。  
  
6.  如果您使用外部供電的集線器，請將集線器的電源線連接到電源插座。  
  
    > [!IMPORTANT]  
    > 我們強烈建議使用已供電的集線器。 不穩定的系統行為可能是因為目前的狀況所造成。  
    >   
    > 使用者不應該將滑鼠和鍵盤直接連接到電腦的 USB 埠。 這麼做可能會導致多個鍵盤和滑鼠的關聯不正確，或完全沒有站。  
  
7.  遵循監視器上出現的指示來建立工作站。  
  
如果您將一個以上的直接視頻連接工作站新增至 MultiPoint 服務環境，主要工作站可能會變更。 您可以輕鬆地找出哪些直接視頻連接的工作站是您的主要工作站。  
  
## <a name="to-find-out-which-direct-video-connected-station-is-the-primary-station"></a>找出哪一個直接連接影片的工作站是主要工作站  
  
1.  開啟直接連線到電腦顯示配接器的所有監視（視訊卡）。  
  
2.  啟動（或重新開機） MultiPoint 服務電腦，並查看哪一個監視器會顯示啟動畫面。 該工作站是主要工作站。  
  
    > [!NOTE]  
    > 在某些情況下，BIOS 啟動資訊會同時在多個監視器上顯示。 在這種情況下，可以將任何監視器視為「主要工作站」，以存取 BIOS。