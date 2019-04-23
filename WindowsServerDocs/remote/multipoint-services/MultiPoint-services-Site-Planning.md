---
title: MultiPoint 服務網站規劃
description: Windows Server 2016 中的 MultiPoint 服務部署的規劃資訊
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 063783cd-d748-489e-b175-46eadc993f7a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 60d2e1fedc018136f5668d55a8afb2e0574d94de
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845069"
---
# <a name="multipoint-services-site-planning"></a>MultiPoint 服務網站規劃
您應該考慮將會部署一或多部執行 MultiPoint 服務和其相關聯的站台的位置。  
  
正在執行 MultiPoint 服務角色的電腦應該已電源供應器，並直接連線到它，例如印表機周邊裝置方便存取。 此外，執行 MultiPoint 服務電腦都必須方便存取的網路連線。 網路連線是需要存取網際網路，以及在何處，LAN。  
  
其他因素需要考量包括下列各項：  
  
-   MultiPoint 服務系統將會在特定的房間內，或將它設定在輪流的 「 購物車 」 或 「 資料表，使其可以移動位置嗎？  
  
    > [!NOTE]  
    > 如果您打算使用行動裝置的安裝程式，您可以*產生關聯*使用 MultiPoint 服務每次您重新連線，以確保每個鍵盤和滑鼠是適當的監視器與相關聯的站台。  
  
-   將其他站台，旁邊位於主要站台或它是不同嗎？ 比方說，如果 MultiPoint 服務系統設定教室中，將主要站台在老師的支援人員，而標準的站台上位於其他地方聊天室嗎？ 重新啟動執行 MultiPoint 服務的電腦時，主要站台會有啟動螢幕的存取。 如果您擔心如果教室裡的存取層級，您可能會想要置於在老師的服務台的主要站台。  
  
-   多少個工作站可容納在聊天室中？  
  
-   您需要的網路嗎？ 使用直接的視訊連線或 usb 極簡型用戶端連線站台的單一伺服器解決方案不需要的網路。  
  
-   有足夠的網路連線來支援所需的執行 MultiPoint 服務的電腦數目，聊天室中  
  
-   電源插座位於何處？  
  
-   您是否需要其他的顯示裝置，例如投影機？ 如果您打算使用投影機時，將它停止回應的上限，或將它放在資料表嗎？  
  
-   就必須使用何種纜線，而且多少必須用嗎？  
  
-   請考慮您可能要將在未來擴大的方式。 將您新增更多站台嗎？  
  
## <a name="station-layout-and-configuration"></a>站台版面配置和組態  
您的站台的實體配置可能會影響您選擇的站台類型。 如需不同站台類型的詳細資訊，請參閱[MultiPoint 站台](MultiPoint-services-Stations.md)本指南中。 單一的 MultiPoint 服務上允許多個站台類型。 這會讓您提供 額外的彈性，以符合您的安裝需求。  
  
### <a name="layout-for-direct-video-connected-stations"></a>直接視訊連線的站台的版面配置  
  
-   直接視訊連線的站台，監視與電腦之間的距離會受到視訊纜線長度而定。  
  
-   使用中繼集線器或菊輪鍊的站台集線器支援的部署輕鬆，但建議使用連續的中樞數目的最大值為 3。 這表示，從電腦到站台集線器的最大距離是 15 公尺，因為每個 USB 2.0 纜線有五個計量的最大長度。  
  
> [!IMPORTANT]  
> 一律應該至少一個直接視訊連線站台每台電腦做為主要站台。  
  
### <a name="layout-for-usb-zero-client-connected-stations"></a>Usb 極簡型用戶端連線的站台的版面配置  
  
-   使用中繼集線器或菊輪鍊的站台集線器支援的部署輕鬆，但建議使用連續的中樞數目的最大值為 3。 這表示，從電腦到站台集線器的最大距離是 15 公尺，因為每個 USB 2.0 纜線有五個計量的最大長度。  
  
-   建議的零個用戶端連接到單一中繼集線器的 USB 數目的上限為三個。  
  
    > [!NOTE]  
    > 某些電腦的主機板上隨附的一般集線器具有加入其他中樞之間的影響*根中樞*電腦和站台集線器。  
  
-   如果會大量使用視訊，則建議不超過兩個 usb 極簡型用戶端與伺服器上的 USB 連接埠。 比方說，如果中繼集線器使用時，只有兩個 usb 極簡型用戶端應該連接到它。 或者，如果您是 daisy 鏈結 usb 極簡型用戶端，只有兩個 USB 零個用戶端應該鏈結在一起。 新增到伺服器上的 USB 連接埠的每個 usb 極簡型用戶端會減少可用的視訊頻寬。  
  
-   如果您打算將葀佹 usb 極簡型用戶端連接到伺服器上的單一 USB 連接埠時，建議使用在伺服器之間中繼集線器的 USB 3.0。  
  
> [!NOTE]  
> 建議您確認使用您的應用程式的效能，並決定多少 USB 硬體零用戶端，您可以連接到伺服器上的 USB 連接埠。  
  
![下游集線器](./media/WMS_diagram6.gif)  
  
**圖 5** MultiPoint 服務系統，具有三個 USB 連線到單一的中繼集線器的零個用戶端  
  
### <a name="layout-for-rdp-over-lan-connected-stations"></a>版面配置 RDP over lan 連線的工作站  
沒有實際距離限制 LAN 用戶端。 只要它們位於區域網路時，它們可以連接到 MultiPoint 服務系統。  
  
## <a name="using-additional-hubs"></a>使用其他中樞  
其他中樞可用來簡化安裝。 有三種類型的 MultiPoint 服務系統所使用的中樞：  
  
-   [站台集線器](#Station-hubs)  
  
-   [中繼集線器](#Intermediate-hubs)  
  
-   [下游集線器](#Downstream-hubs)  
  
### <a name="station-hubs"></a>站台集線器  
站台集線器是 MultiPoint 服務站相關聯的外部中樞。 至少，站台集線器會有鍵盤插入入它。 它也有其他的週邊設備連接。 站台集線器可以是一般的 USB 集線器的 USB 2.0 或更新版本的規格的符合。 如果強大的裝置將會以它們的外掛程式，就應該從外部供電站台集線器。  
  
**根集線器**就是內建於主機上的控制站電腦的主機板的 USB 集線器*根中樞*。 站台集線器是通常插入單元來執行 MultiPoint 服務的電腦上的根中樞。  
  
> [!NOTE]  
> 根中樞不應該作為站台集線器。 內建至電腦 USB 連接埠時，通常不可能以判斷它們在內部連線到哪一個 USB 根中樞。 同樣地，如果您已插入之站台將鍵盤和滑鼠直接與電腦的 USB 連接埠，您可能實際上是插入單元鍵盤和滑鼠，以不同的 USB 根中樞。 保證鍵盤和滑鼠在同一個中樞外掛程式到電腦的 USB 連接埠，並接著外掛程式的站台集線器的鍵盤和滑鼠，站台集線器。  
  
**鏈結站台的 daisy**可能是您更輕鬆地連接至另一個站台集線器，而不是直接與電腦的站台集線器。 這可讓您的站台集線器是已插入入到電腦上，連接的 USB 集線器，好讓您有附加至另一個站台集線器站台集線器。  
  
應該不超過三個 usb 極簡型用戶端或站台集線器 daisy 連續鏈結。 必須小心 USB 頻寬未超過當 daisy 鏈結站台集線器。  
  
![菊輪鍊工作站](./media/WMS_diagram5.gif)  
  
**圖 6**與菊輪鍊工作站的 MultiPoint 服務系統  
  
### <a name="intermediate-hubs"></a>中繼集線器  
中繼集線器是在伺服器之間的站台集線器的中樞。 它通常用來增加可用的站台集線器連接埠的數目，或從電腦延伸的站台的距離。 建議您不要超過兩個中繼集線器，會使用站台集線器與伺服器之間。  
  
中繼集線器的 USB 2.0 或更新版本，必須和必須從外部開啟它們。 USB 3.0 建議伺服器與中繼的中樞之間，如果您要連接到中繼集線器的三個以上的 usb 極簡型用戶端。  
  
### <a name="downstream-hubs"></a>下游集線器  
下游集線器連接到站台集線器，以新增更多可用的站台裝置的連接埠。 下游集線器可以是外部供電或匯流排供電，根據已插入之中樞的裝置。  
  
![多個 usb 極簡型用戶端連線](./media/WMS_diagram4.gif "WMS_diagram4")  
  
**圖 7**中繼集線器、 站台集線器，與下游集線器的 MultiPoint 服務系統  
  
## <a name="users-stations-and-computers"></a>使用者、 電腦及站台  
您需要的站台數目取決於必須存取在同一時間執行 MultiPoint 服務電腦的人的數目。 同樣地，執行您需要的 MultiPoint 服務的電腦數目取決於所需的站台的總數。 直接視訊連線的站台、 USB 零-用戶端-連接站台和 RDP-over-LAN 連接的站台是所有考量的站台。 此外，如果使用分割畫面的功能，每半視為站台。  
  
## <a name="power-considerations"></a>能力的考量  
下列元件所需的電源線或輸出的存取權：  
  
-   Server  
-   [監視]
-   中繼集線器\(如果使用\) 
-   某些 usb 極簡型用戶端  
-   供電的 USB 裝置，例如某些外部存放裝置和 DVD 光碟機  
  
## <a name="sample-multipoint-services-system-layouts"></a>範例 MultiPoint 服務系統配置  
根據可用的家具房間的大小、 正在執行 MultiPoint 服務，而站台在聊天室中的電腦數目，有各種不同的實體站台，可排列方式。 下圖說明五個可能的替代方案。  
  
> [!NOTE]  
> 部分這些圖表顯示投影機連接到 MultiPoint 服務系統。 這只是舉例;包括投影機 MultiPoint 服務系統中是選擇性的。  
  
**電腦實驗室**這項設定，在站台的排列方式排列周圍的空間，背景牆面向的背景牆的學生。  
  
![電腦實驗教室設定](./media/WMS_ComputerLabLayout.gif)  
  
**群組**在此設定中，有三部電腦執行 MultiPoint 服務以解決每一部電腦叢集的站台。  
  
![使用伺服器 Pod 設定的教室](./media/WMS_ClassroomPods.gif)  
  
**Lecture 聊天室**這項設定，在站台設定中的資料列。 此設定的優點是所有的學生所面臨的講師。  
  
![設定為演講廳的教室](./media/WMS_LectureRoom.gif)  
  
**活動中心**這個安裝程式所組成的桌子，為傳統的演講廳的版面配置，而且它有個別的區域以其相關聯的站台執行 MultiPoint 服務的單一電腦。  
  
![MultiPoint 服務活動中心](./media/WMSActivityCenter.gif)  
  
**小型企業辦公室**這項設定，在執行 MultiPoint 服務電腦放置在中央位置，並使用區域網路整個辦公室的使用者連接到它\(LAN\)。  
  
![USB 極簡型用戶端連線的工作站](./media/Diagram1.gif)