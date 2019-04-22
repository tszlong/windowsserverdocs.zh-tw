---
title: 設定實體電腦與主要站台
description: 了解如何設定您的第一個系統，MultiPoint 服務中的主要站台
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e83b126-ce9a-4cd7-a0bd-6627c9e0f81b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 6569c4963d31b72216943bf29b71411e702caf84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812009"
---
# <a name="set-up-the-physical-computer-and-primary-station"></a>設定實體電腦與主要站台
在您安裝 MultiPoint 服務之前，您需要設定您的 MultiPoint 服務系統的主要站台。 如果您將使用區域網路 (LAN) 將電腦連線到 LAN。  
  
A*站台*是由此存取 MultiPoint 服務的端點。 *主要站台*是第一次的站台，以啟動時啟動 MultiPoint 服務。 系統管理員可以使用它存取啟動功能表和設定。 主要站台提供的存取權的系統組態和疑難排解資訊，而且只可在啟動期間之前 MultiPoint 服務系統正在執行。 啟動之後，您可以使用主要站台，如同任何其他站台。  
  
主要站台必須是直接視訊連線的站台。 下列程序描述如何連接到 MultiPoint 服務電腦的所需的硬體。  
  
如需有關站台的詳細資訊，請參閱 < [MultiPoint 站台](multipoint-services-stations.md)。 使用硬體選擇完畢的協助，請參閱[選取您的 MultiPoint 服務系統的硬體](Selecting-Hardware-for-Your-MultiPoint-services-System.md)。 如需連線資訊其他廣播站 MultiPoint 服務的類型，請參閱 <<c0> [ 連接到 MultiPoint 服務電腦的其他站台](Attach-additional-stations-to-your-MultiPoint-services-computer.md)。  
  
> [!NOTE]  
> 若要建立的視訊連接站台，您必須使用拉丁文鍵盤 （例如英文或西班牙文語言 keyboard)。  
  
## <a name="to-set-up-your-primary-station"></a>若要設定您的主要站台  
  
1.  請確定執行 MultiPoint 服務的電腦已關閉，而且已拔除。  
  
2.  監視器的電源線接上電源插座，並連接監視器纜線連接到電腦上的視訊顯示連接埠，如下所示。  
  
    ![視訊連線到 USB 集線器架構系統的影像](./media/WMSVideoConnection.gif)  
  
3.  如果 USB 鍵盤和滑鼠，將使用站台，請完成下列步驟：  
  
    1.  連上外接式 USB 集線器的電腦上，開啟的 USB 連接埠，如下所示。  
  
        ![MultiPoint 服務 USB 集線器連線的影像](./media/WMSUSBHubConnection.gif)  
  
    2.  連接到 USB 集線器的 USB 鍵盤和滑鼠。  
  
        ![USB 集線器輸入裝置連線的影像](./media/WMSUSBDeviceConnection.gif)  
  
        > [!NOTE]  
        > 如果您的 MultiPoint 服務電腦有 PS/2 連接埠，您可以如有需要使用 PS/2 將鍵盤和滑鼠直接插入電腦。 不過，此設定具有諸多限制。 使用者無法使用的音訊裝置、 web 隨身攝影機，，和快閃磁碟機，在 PS/2 的站台上。  
  
    3.  如果您使用外部供電的集線器，中樞的電源線接上電源插座。  
  
        > [!IMPORTANT]  
        > 我們強烈建議使用供電的集線器。 不足的目前條件可能不穩定的系統行為。  
        >   
        > 使用者應該不滑鼠及鍵盤直接附加到電腦的 USB 連接埠。 這樣做很可能完全造成不正確的多個鍵盤和滑鼠相同的站台，或沒有站台關聯。  
  
        > [!NOTE]  
        > MultiPoint 服務處於主控台模式時，才可使用系統主機板上的主應用程式音訊裝置。 若要確保不中斷的音訊會使用外接式 USB 集線器站台，您必須使用一個插入集線器的 USB 音訊裝置。  
  
## <a name="to-connect-the-computer-to-the-lan"></a>若要將電腦連線到 LAN  
  
-   如果您有區域網路，請將電腦連線到您網路的網路纜線。