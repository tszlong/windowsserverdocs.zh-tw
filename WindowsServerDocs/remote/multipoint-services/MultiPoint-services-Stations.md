---
title: MultiPoint 站台
description: 深入了解站台，MultiPoint 服務，包括使用者的不同選項
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9f9d618-ccfe-41ea-a52c-00c3c7adb51a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: e747826a7cd84521bc62e48abedf3092bf6d844c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855649"
---
# <a name="multipoint--stations"></a>MultiPoint 站台
在 MultiPoint 服務系統環境中，*站台*是連線到執行 MultiPoint 服務電腦的使用者端點。 每個站台會提供獨立的 Windows 10 體驗的使用者。 支援下列的站台類型：  
  
-   直接視訊連線的站台  
  
-   USB 零-用戶端-連接的站台 （包括 USB over Ethernet 零用戶端）  
  
-   RDP-over-LAN 連接的站台 （適用於豐富型用戶端或精簡型用戶端電腦）  
  
已安裝的 MultiPoint 連接器的完整電腦也可以監視及控制使用 MultiPoint 儀表板。 在 Windows 10 上，可以透過控制台中的 Windows 功能啟用 MultiPoint 連接器。 

Multipoint 服務支援任何類型的組合這些站台，但建議一個站台是直接視訊連線站台，其可做為主要站台。 這項建議的原因是要能夠預期支援案例。 比方說，若要與系統互動的 BIOS 之前正在執行 MultiPoint 服務。  
  
## <a name="primary-stations-and-standard-stations"></a>主要站台，而標準的站台  
一個直接視訊連線的站台指*主要站台*。 其餘的站台指*標準的站台*。  
  
當您開啟電腦時，主要站台就會顯示啟動畫面。 它提供系統組態和疑難排解在啟動期間才可使用的資訊存取權。 主要站台必須是直接視訊連線的站台。 啟動之後，您可以使用主要站台，像任何其他 MultiPoint 站台。  
  
## <a name="direct-video-connected-stations"></a>直接視訊連線的站台  
執行 MultiPoint 服務的電腦可以包含多個視訊卡，每一個都可以有一個以上的視訊連接埠。 這可讓您監視多個站台直接插入電腦。 透過每個監視相關聯的 USB 集線器連線鍵盤及滑鼠。 這些中樞指*站台集線器*。 其他週邊裝置，例如喇叭、 耳機或 USB 存放裝置也可以連線到站台集線器，並僅供該工作站的使用者。  
  
> [!IMPORTANT]  
> 應該有至少一個*直接視訊連線的站台*每一部伺服器做為主要的站台，以開啟電腦時，顯示在啟動程序。  
  
![MultiPoint 服務 USB 架構系統配置的影像](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
**圖 1** MultiPoint 服務具有四個直接視訊連線的站台系統  
  
### <a name="BKMK_PS2stations"></a>PS/2 的站台  
使用 MultiPoint 服務，您可以將 PS/2 鍵盤和滑鼠在主機板上的對應建立 PS/2 站台直接的視訊連接監視器。 邴畫觶主機板上的類比音訊是這種類型的站台相關聯的音訊。 這不會套用到電腦的主機板上有沒有 PS/2 端子。  
  
## <a name="usb-zero-client-connected-stations"></a>USB 零-用戶端-連接站台  
0-用戶端-已連接 USB 電台運用*usb 極簡型用戶端*作為站台集線器。 Usb 極簡型用戶端有時也稱為 「 多功能集線器與視訊 」。 他們會連線到透過 USB 纜線，電腦的中樞，通常這些中樞支援的視訊監視器、 滑鼠和鍵盤 （PS/2 或 USB） 音訊及其他 USB 裝置。 本指南將這些特製化中樞稱為 USB 零個用戶端。  
  
下圖顯示與主要站台的 MultiPoint server 系統 （直接的視訊連線的工作站） 和兩個額外的 usb 極簡型用戶端連線的工作站。  
  
![USB 零-用戶端連線的工作站](./media/WMS11_diagram7.gif)  
  
**圖 2** MultiPoint 服務系統的主要站台與兩個 USB 零用戶端連線站台  
  
### <a name="usb-over-ethernet-zero-clients"></a>USB over Ethernet 零用戶端  
USB over Ethernet 零用戶端是透過 LAN 傳送 USB，到 MultiPoint 服務系統的 usb 極簡型用戶端的變化。 這些類型的 usb 極簡型用戶端的功能類似其他 usb 零的用戶端，但不是會受到 USB 纜線長度最大值。 USB over Ethernet 零用戶端不是傳統的精簡型用戶端，而且它們會顯示為 MultiPoint 服務系統上的虛擬 USB 裝置。 當使用這些裝置，請參閱裝置製造商，以取得特定的效能和計劃建議的站台。 大部分的裝置具有 MultiPoint 管理員，可讓您產生關聯，並將裝置連接到 MultiPoint 服務系統的協力廠商外掛程式。  
  
## <a name="rdp-over-lan-connected-stations"></a>RDP over LAN 連線的工作站  
精簡型用戶端和傳統的桌上型電腦、 膝上型電腦或平板電腦可以連線到使用遠端桌面通訊協定 (RDP) 或專屬的通訊協定和遠端桌面通訊協定，透過區域網路 (LAN) 執行 MultiPoint 服務的電腦提供者。 透過 RDP 連線提供任何其他 MultiPoint 站台，非常類似的終端使用者體驗，但會使用本機用戶端電腦的硬體。 深入了解遠端桌面應用程式可用的 Android、 iOS、 Mac 和 Windows 中的[遠端桌面用戶端](../remote-desktop-services/clients/remote-desktop-clients.md)。 
  
用戶端與執行 Microsoft RemoteFX 的裝置可以利用處理器和視訊的硬體功能在本機的精簡型用戶端或電腦透過網路提供高畫質視訊提供豐富的多媒體體驗。  
  
如果您有現有的 LAN 用戶端 MultiPoint 服務可以提供快速且符合成本效益的方式，同時將所有使用者升級至 Windows 10 體驗。  
  
部署和管理的觀點而言，當您使用 RDP-over-LAN 連接的站台時存在下列差異：  
  
-   不限於實體的 USB 連接距離  
  
-   要以站台重複使用舊型電腦硬體的潛能  
  
-   更輕鬆地調整為較大的站台。 在您網路上的任何用戶端可能可用來當做遠端站台  
  
-   沒有任何硬體透過 MultiPoint 管理員主控台進行疑難排解  
  
-   沒有分割畫面的功能。  
  
    如需詳細資訊，請參閱 <<c0> [ 分割畫面工作站](#a-namebkmksplitscreenstationsasplit-screen-stations)本主題稍後的  
  
-   任何站台重新命名或透過 MultiPoint 管理員主控台的設定自動登入  
  
![USB 極簡型用戶端連線的工作站](./media/Diagram1.gif)  
  
**圖 3** RDP-over-LAN 連接的電台的 MultiPoint 服務系統  
  
## <a name="additional-configuration-options"></a>其他組態選項  
  
### <a name="BKMK_SplitscreenStations"></a>分割畫面工作站  
MultiPoint 服務會提供直接視訊連線的站台或 USB 零-用戶端-連接站台的電腦上的分割畫面 選項。 分割畫面會提供建立每個監視的其他站台的能力。 而不需要兩個監視器，您可以使用一部監視器有兩個站台集線器設定，建立兩個站台與一部監視器。 您可以快速提升可用的站台數目，而不必購買額外的監視器、 USB 免用戶端或視訊卡。  
  
使用分割畫面站台的優點可以包括：  
  
-   降低成本和空間容納更多使用者的 MultiPoint 服務系統上。  
  
-   讓兩個使用者共同作業專案並存。  
  
-   允許老師向示範站台上的程序，而在學生遵循其他站台。  
  
任何 MultiPoint 服務站台，都有 1024 x 768 解析度或更高的可分成兩個站台畫面的監視。 為了最佳的分割畫面使用者體驗，建議使用具有最小的 1600x900 解析的寬螢幕。 沒有數字鍵台迷你鍵盤也建議您允許符合監視器前面兩個鍵盤。  
  
若要建立分割畫面站台，您設定一個直接視訊連線或 USB 零-用戶端-連接的站台。 然後您可以新增額外的站台集線器插入鍵盤和滑鼠連接到伺服器的 USB 集線器。 您還可以使用分割畫面，並將新的中樞對應到此監視的下半部的 MultiPoint 管理員，將站台然後轉換成兩個站台。 畫面左的半邊變成一個站台，而右半部變成第二個站台。  
  
分割站台之後，一位使用者可以登入左側的站台，而另一個使用者登入正確的站台。  
  
![分割畫面工作站](./media/WMS_diagram3.gif)  
  
**圖 4**分割畫面工作站與 MultiPoint 服務系統  
  
## <a name="BKMK_StationTypeComparison"></a>站台類型比較  
  
||直接連接的影片|Usb 極簡型用戶端連線|RDP 移轉網路連線|  
|-|--------------------------|-----------------------------|----------------------------|  
|影片的效能|為了達到最佳的視訊效能建議||使用支援 RemoteFX，以提高視訊品質，在較低的網路頻寬的精簡型用戶端|  
|實體的限制|受限於視訊纜線長度和 USB 集線器和纜線長度 （建議使用 15 計量表最大長度）|受限於 USB 集線器和纜線長度 （建議使用 15 計量表最大長度）|受限於區域網路發佈|  
|允許的站台數目 |在主機板上的可用 PCIe 插槽數目受限於逾每個視訊卡的視訊連接埠|Usb 極簡型用戶端製造商可能會受限於總數 （如需詳細資訊，請參閱此表格後面的附註）。|受限於網路交換器上的可用連接埠|  
|分割畫面|是|是|否|  
|MultiPoint 管理員站台週邊狀態，自動登入設定站台重新命名|是|是|否|  
|存取伺服器啟動功能表|是|否|否|  
  
> [!NOTE]  
> 連接到伺服器的 usb 極簡型用戶端總數可能會受限於製造商或執行 MultiPoint 服務之電腦的硬體功能。