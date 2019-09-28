---
title: 設定實體電腦與主要站台
description: 瞭解如何在 MultiPoint 服務中設定您的第一個系統（主要工作站）
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e83b126-ce9a-4cd7-a0bd-6627c9e0f81b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 1a5865b6bd15b6cd07cde393012afd495e3378be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395294"
---
# <a name="set-up-the-physical-computer-and-primary-station"></a>設定實體電腦與主要站台
安裝 MultiPoint 服務之前，您必須先設定 MultiPoint 服務系統的主要工作站。 如果您將使用區域網路（LAN），請將電腦連線到 LAN。  
  
「*工作站*」是用來存取 MultiPoint 服務的端點。 當 MultiPoint 服務啟動時，*主要工作站*是第一部要啟動的工作站。 系統管理員可以使用它來存取啟動功能表和設定。 主要工作站會提供系統組態和疑難排解資訊的存取權，只有在啟動期間和 MultiPoint 服務系統執行時才可使用。 啟動之後，您可以使用主要工作站，就像任何其他工作站一樣。  
  
主要工作站必須是直接視頻連線的工作站。 下列程式說明如何將所需的硬體連接到 MultiPoint 服務電腦。  
  
如需有關工作站的詳細資訊，請參閱[MultiPoint 電臺](multipoint-services-stations.md)。 如需進行硬體選擇的說明，請參閱[選取 MultiPoint 服務系統的硬體](Selecting-Hardware-for-Your-MultiPoint-services-System.md)。 如需將其他工作站類型連接到 MultiPoint 服務的相關資訊，請參閱[將其他工作站連接到 Multipoint 服務電腦](Attach-additional-stations-to-your-MultiPoint-services-computer.md)。  
  
> [!NOTE]  
> 若要建立與視頻連接的工作站，您必須使用拉丁鍵盤（例如英文或西班牙文語言鍵盤）。  
  
## <a name="to-set-up-your-primary-station"></a>若要設定您的主要工作站  
  
1.  確定執行 MultiPoint 服務的電腦已關閉並將其拔下。  
  
2.  將監視器的電源線連接到電源插座，並將監視器纜線連接到電腦上的影片顯示埠，如下所示。  
  
    ![視訊連線到 USB 集線器架構系統的影像](./media/WMSVideoConnection.gif)  
  
3.  如果工作站會使用 USB 鍵盤和滑鼠，請完成下列步驟：  
  
    1.  將外部 USB 集線器連接到電腦上已開啟的 USB 埠，如下所示。  
  
        ![MultiPoint 服務 USB 集線器連線的影像](./media/WMSUSBHubConnection.gif)  
  
    2.  將 USB 鍵盤和滑鼠連接到 USB 集線器。  
  
        ![USB 集線器輸入裝置連線的影像](./media/WMSUSBDeviceConnection.gif)  
  
        > [!NOTE]  
        > 如果您的 MultiPoint 服務電腦具有 PS/2 埠，您可以視需要使用 PS/2 鍵盤，並將滑鼠直接插入電腦中。 不過，這種設定有很大的限制。 使用者無法在 PS/2 站上使用音訊裝置、web 攝影機和快閃磁片磁碟機。  
  
    3.  如果您使用外部供電的集線器，請將集線器的電源線連接到電源插座。  
  
        > [!IMPORTANT]  
        > 我們強烈建議使用已供電的集線器。 不穩定的系統行為可能是因為目前的狀況所造成。  
        >   
        > 使用者不應該將滑鼠和鍵盤直接連接到電腦的 USB 埠。 這麼做可能會導致多個鍵盤和滑鼠的關聯不正確，或完全沒有站。  
  
        > [!NOTE]  
        > 只有當 MultiPoint 服務處於主控台模式時，才能使用系統主機板上的主機音訊裝置。 若要確保使用外部 USB 集線器之工作站的音訊不中斷，您必須使用插入集線器的 USB 音訊裝置。  
  
## <a name="to-connect-the-computer-to-the-lan"></a>將電腦連線到 LAN  
  
-   如果您有 LAN，請使用網路纜線將您的電腦連接到網路。