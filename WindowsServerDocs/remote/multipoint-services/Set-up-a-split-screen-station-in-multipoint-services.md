---
title: 設定分割畫面站台
description: 描述如何設定 MultiPoint 服務，讓兩位使用者可以共用單一系統
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d1d434-79b2-4e0a-835f-d94ed8ee6b21
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b2d01df6175eee99fd1374cd5af270cbcc764ca8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849939"
---
# <a name="set-up-a-split-screen-station"></a>設定分割畫面站台
您可以設定分割畫面站台，讓兩位使用者都能同時使用系統。

任何時連線到站台可支援分割畫面 功能中，有的最小 x 720 的 1200年解析度的監視器可以分割成兩個站台。 分割站台之後，監視器必須顯示在桌面上移動螢幕的左半邊和新的站台會顯示在螢幕的右半部。 若要完成建立新的站台之後，您必須對應鍵盤、 滑鼠及 USB 集線器站台。 分割站台後，使用者可以登入到左方站台，而另一個使用者則可登入右方站台。  
  
分割畫面站台有數個優點：  
  
-   您可以降低成本和空間容納更多使用者的 MultiPoint 服務系統上。  
  
-   兩個使用者可以在一起，並排顯示，專案上共同作業。  
  
-   MultiPoint 儀表板使用者可以示範站台上的程序，而在學生遵循其他站台。  
  
下圖顯示 （右側） 分割畫面站台的 MultiPoint 服務系統。  
  
![分割畫面工作站](./media/WMS_diagram3.gif)  
   
## <a name="requirements-for-a-split-screen-station"></a>分割畫面站台的需求  
若要建立分割畫面站台，監視器 和 站台必須符合下列需求：  
  
-   此監視必須具有的 1200年解析度 x720 或更高版本。  
  
-   如果您使用 USB over Ethernet 零用戶端，請洽詢您的硬體廠商，以了解是否支援分割畫面站台。 許多 USB over Ethernet 零用戶端裝置必須防止其設定為分割畫面站台的限制。  
  
## <a name="setting-up-a-split-screen-station"></a>設定分割畫面站台  
新增第二個中樞的分割畫面站台，並再分割 MultiPoint 服務中的站台使用下列程序。 最後的程序說明如何分割畫面站台回到單一站台。  
  
> [!NOTE]  
> 當您分割站台時，會暫停站台上使用中的工作階段。 使用者必須登入一次之後的分岔會出現,，請繼續工作的站台。  
  
**若要新增鍵盤和滑鼠的第二個中樞：**  
  
1.  下圖所示，請連接到的電腦上，開啟的 USB 連接埠的 USB 集線器。  
  
    ![MultiPoint server USB 集線器連線的映像](./media/WMSUSBHubConnection.gif)  
  
2.  連接到 USB 集線器的鍵盤和滑鼠。  
  
    ![USB 集線器輸入裝置連線的影像](./media/WMSUSBDeviceConnection.gif)  
  
3.  連接任何其他的週邊設備，例如耳機到 USB 集線器。  
  
4.  如果您使用外部供電的集線器，中樞的電源線接上電源插座。  
  
**分割站台：**  
  
1.  在 MultiPoint 管理員中，按一下**站台** 索引標籤。  
  
2.  底下**站台**，按一下您要分割的站台名稱。  
  
3.  底下**選取項目工作**，按一下**分割站台**。  
  
    原始的畫面移至左半部監視器，並建立新的站台的螢幕的右半部相同監視器上。  
  
4.  建立新的站台時所示，新增的鍵盤上按指定的信件**建立 MultiPoint Server 站台**的右半部監視器上出現的畫面。  
  
分割站台之後，一位使用者可以登入左側的站台，而另一個使用者登入正確的站台。  
  
**若要返回單一站台的分割站台復原：**  
  
1.  在 MultiPoint 管理員中，按一下**站台** 索引標籤。  
  
2.  底下**站台**，按一下您想要 unsplit 站台的名稱。  
  
3.  底下**選取項目工作**，按一下**取消分割站台**。