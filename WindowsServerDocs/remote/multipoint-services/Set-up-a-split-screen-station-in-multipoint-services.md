---
title: 設定分割畫面站台
description: 描述如何設定 MultiPoint 服務，讓兩個使用者可以共用單一系統
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
ms.openlocfilehash: a0f1ea32112865c7120a3fe0af0c9f413032a32e
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871589"
---
# <a name="set-up-a-split-screen-station"></a>設定分割畫面站台
您可以設定分割畫面的工作站，讓兩個使用者同時使用系統。

當連接到支援分割畫面功能的工作站時，任何解析度最低為 1200 x720 的監視器，都可以分割成兩個工作站。 當工作站被分割之後，監視器顯示的桌面會移到畫面的左邊半部，而新的工作站會顯示在畫面的右半部。 若要完成新工作站的建立，您必須將鍵盤、滑鼠和 USB 集線器對應到工作站。 分割站台後，使用者可以登入到左方站台，而另一個使用者則可登入右方站台。  
  
分割螢幕電臺有數個優點：  
  
-   您可以藉由在 MultiPoint 服務系統上容納更多使用者，來降低成本和空間。  
  
-   兩位使用者可以在專案上並排共同作業。  
  
-   MultiPoint 儀表板使用者可以在同一部工作站上示範程式，而學生也會在另一部站上進行操作。  
  
下圖顯示具有分割螢幕工作站（右側）的 MultiPoint 服務系統。  
  
![分割畫面工作站](./media/WMS_diagram3.gif)  
   
## <a name="requirements-for-a-split-screen-station"></a>分割螢幕工作站的需求  
若要建立分割螢幕工作站，監視器和工作站必須符合下列需求：  
  
-   此監視器必須具有 1200 x720 或更高版本的解決方案。  
  
-   如果您使用 USB over Ethernet 的零用戶端，請洽詢您的硬體廠商，以瞭解是否支援分割螢幕電臺。 許多 USB over 乙太網路的零用戶端裝置有限制，因此無法將其設定為分割螢幕工作站。  
  
## <a name="setting-up-a-split-screen-station"></a>設定分割畫面的工作站  
使用下列程式，為分割畫面的工作站新增第二個中樞，然後在 MultiPoint 服務中分割該站。 最後的程式說明如何將分割畫面的工作站傳回給單一工作站。  
  
> [!NOTE]  
> 當您分割站台時，會暫停站台上使用中的工作階段。 使用者必須在進行分割之後，再次登入工作站以繼續工作。  
  
**使用鍵盤和滑鼠新增第二個中樞：**  
  
1.  將 USB 集線器連接到電腦上已開啟的 USB 埠，如下圖所示。  
  
    ![MultiPoint server USB 集線器連線的影像](./media/WMSUSBHubConnection.gif)  
  
2.  將鍵盤和滑鼠連接到 USB 集線器。  
  
    ![USB 集線器輸入裝置連線的影像](./media/WMSUSBDeviceConnection.gif)  
  
3.  將任何其他週邊設備（例如耳機）連接到 USB 集線器。  
  
4.  如果您使用外部供電的集線器，請將集線器的電源線連接到電源插座。  
  
**若要分割站：**  
  
1.  在 MultiPoint 管理員中，按一下 [**工作站**] 索引標籤。  
  
2.  在 [**工作站**] 底下，按一下您要分割的工作站名稱。  
  
3.  在 [**選取的專案**工作] 底下，按一下 [**分割站**]。  
  
    原始畫面會移到監視器的左邊半部，而新的工作站畫面會建立在相同監視器的右半部。  
  
4.  在新加入的鍵盤上按下指定的字母，然後在監視器的右半部出現 [**建立 MultiPoint 伺服器工作站**] 畫面時，建立新的工作站。  
  
在工作站被分割之後，一位使用者可以在另一位使用者登入正確的工作站時，登入左側的工作站。  
  
**若要將分割站交還給單一工作站：**  
  
1.  在 MultiPoint 管理員中，按一下 [**工作站**] 索引標籤。  
  
2.  在 [**工作站**] 底下，按一下您想要解除的工作站名稱。  
  
3.  在 [**選取的專案**工作] 底下，按一下 [**解除站**]。