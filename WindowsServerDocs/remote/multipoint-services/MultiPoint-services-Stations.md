---
title: MultiPoint 站台
description: 瞭解 MultiPoint 服務中的工作站，包括不同的使用者選項
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
ms.openlocfilehash: 4aa08f58f8fdf6d6fce816ee090275b0bf46a844
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871633"
---
# <a name="multipoint--stations"></a>MultiPoint 工作站
在 MultiPoint 服務系統內容中，*工作站*是用來連接到執行 MultiPoint 服務之電腦的使用者端點。 每個工作站會為使用者提供獨立的 Windows 10 體驗。 支援下列工作站類型：  
  
-   直接連接視頻的工作站  
  
-   USB-零-用戶端連線的工作站（包括 USB over Ethernet 的零用戶端）  
  
-   透過區域網路連線的工作站（適用于豐富型用戶端或瘦用戶端電腦）  
  
已安裝 MultiPoint 連接器的完整電腦也可以使用 MultiPoint 儀表板來監視和控制。 在 Windows 10 上，您可以透過 Windows 功能的 [控制台] 來啟用 MultiPoint 連接器。 

Multipoint 服務支援這些工作站類型的任意組合，但建議一部工作站是直接視頻連線的工作站，可作為主要工作站。 這項建議的原因是要能夠預測支援案例。 例如，在執行 MultiPoint 服務之前，與系統的 BIOS 互動。  
  
## <a name="primary-stations-and-standard-stations"></a>主要工作站和標準工作站  
一個直接連線到視頻的工作站會定義為*主要工作站*。 其餘的工作站則稱為*標準電臺*。  
  
當電腦開啟時，主要工作站會顯示啟動畫面。 它可讓您存取僅供啟動時使用的系統組態和疑難排解資訊。 主要工作站必須是直接視頻連線的工作站。 啟動之後，您可以使用主要工作站，就像任何其他 MultiPoint 工作站一樣。  
  
## <a name="direct-video-connected-stations"></a>直接連接視頻的工作站  
執行 MultiPoint 服務的電腦可以包含多張視訊卡，其中每個都可以有一或多個視訊連接埠。 這可讓您將多部工作站的監視器直接插入電腦中。 鍵盤和滑鼠會透過與每個監視器相關聯的 USB 集線器來連線。 這些中樞稱為「*站中樞*」。 其他週邊裝置（例如喇叭、耳機或 USB 存放裝置）也可以連接到工作站集線器，而且只能供該工作站的使用者使用。  
  
> [!IMPORTANT]  
> 電腦開啟時，每一部伺服器應該至少有一個*直接視頻連線的工作站*，以作為主要工作站以顯示啟動進程。  
  
![MultiPoint 服務以 USB 為基礎的系統版面配置影像](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
**圖 1**MultiPoint 服務系統，具有四個直接視頻連線的工作站  
  
### <a name="BKMK_PS2stations"></a>PS/2 電臺  
使用 MultiPoint 服務時，您可以將主機板上的 PS/2 鍵盤和滑鼠對應到直接連線到視頻的監視器，以建立 PS/2 站。 主機板上的高定義類比音訊是與這種類型的工作站相關聯的音訊。 這不適用於主機板上沒有 PS/2 插座的電腦。  
  
## <a name="usb-zero-client-connected-stations"></a>USB-零-用戶端連線的工作站  
USB-零用戶端連線的工作站會利用*usb 零用戶端*做為工作站中樞。 USB 零用戶端有時稱為具有影片的多功能集線器。 它們是透過 USB 纜線連接到電腦的中樞，而這些中樞通常支援視頻監視器、滑鼠和鍵盤（PS/2 或 USB）、音訊和其他 USB 裝置。 本指南將這些特製化的中樞稱為 USB 零用戶端。  
  
下圖顯示具有主要工作站（直接視頻連接的站）和兩個額外 USB 零用戶端連線站的 MultiPoint server 系統。  
  
![USB 零用戶端連線的工作站](./media/WMS11_diagram7.gif)  
  
**圖 2**具有主要工作站和兩個 USB 零用戶端連線工作站的 MultiPoint 服務系統  
  
### <a name="usb-over-ethernet-zero-clients"></a>USB-乙太網路的零用戶端  
USB over 乙太網路的零用戶端是一種 USB 零用戶端的變化，會將 USB over LAN 傳送至 MultiPoint 服務系統。 這些類型的 USB 零用戶端的運作方式類似于其他 USB 零用戶端，但不受 USB 纜線長度上限的限制。 USB over 乙太網路的零用戶端不是傳統的瘦用戶端，而是顯示為 MultiPoint 服務系統上的虛擬 USB 裝置。 使用這些裝置時，請參閱裝置製造商，以取得特定的效能和網站規劃建議。 大部分的裝置都有適用于 MultiPoint 管理員的協力廠商外掛程式，可讓您將裝置與 MultiPoint 服務系統建立關聯和連接。  
  
## <a name="rdp-over-lan-connected-stations"></a>RDP over LAN 連線的工作站  
瘦用戶端和傳統桌上型電腦、膝上型電腦或平板電腦，都可以使用遠端桌面通訊協定（RDP）或專屬通訊協定和遠端桌面通訊協定，透過區域網路（LAN）連接到執行 MultiPoint 服務的電腦。那裡. RDP 連線提供的使用者體驗與任何其他 MultiPoint 工作站非常類似，但會使用本機用戶端電腦的硬體。 深入瞭解[遠端桌面用戶端](../remote-desktop-services/clients/remote-desktop-clients.md)中適用于 Android、IOS、Mac 和 Windows 的遠端桌面應用程式。 
  
執行 Microsoft RemoteFX 的用戶端和裝置可以利用本機瘦用戶端或電腦的處理器和視頻硬體功能，透過網路提供高定義的影片，來提供豐富的多媒體體驗。  
  
如果您有現有的 LAN 用戶端，MultiPoint 服務可以提供快速且符合成本效益的方式，將所有使用者同時升級至 Windows 10 體驗。  
  
從部署和系統管理的觀點來看，當您使用由 RDP over LAN 連線的工作站時，會有下列差異：  
  
-   不限於實體 USB 連接距離  
  
-   可能重複使用舊版電腦硬體做為工作站  
  
-   更容易調整為更高的工作站數目。 您網路上的任何用戶端都可能做為遠端工作站使用  
  
-   透過 MultiPoint 管理員主控台進行硬體疑難排解  
  
-   沒有分割畫面功能。  
  
    如需詳細資訊，請參閱本主題稍後的[分割畫面電臺](#split-screen-stations)  
  
-   無站重新命名或透過 MultiPoint 管理員主控台設定自動登入  
  
![USB 極簡型用戶端連線的工作站](./media/Diagram1.gif)  
  
**圖 3**具有 RDP over LAN 連線工作站的 MultiPoint 服務系統  
  
## <a name="additional-configuration-options"></a>其他設定選項  
  
### <a name="split-screen-stations"></a>分割畫面工作站  
MultiPoint 服務會在具有直接視頻連線的工作站或 USB-零用戶端連線工作站的電腦上提供分割畫面選項。 分割畫面提供每個監視器建立額外工作站的能力。 除了需要兩個監視器以外，您還可以使用一個具有兩個工作站中樞的監視器來建立具有一個監視器的兩個工作站。 您可以快速增加可用的工作站數目，而不需要購買額外的監視器、USB-零用戶端或視訊卡。  
  
使用分割螢幕工作站的優點包括：  
  
-   藉由在 MultiPoint 服務系統上容納更多使用者，降低成本和空間。  
  
-   允許兩位使用者在專案上並存共同作業。  
  
-   允許老師在一台工作站上示範程式，而學生在另一部工作站上跟著操作。  
  
任何具有1024x768 解析度或以上的 MultiPoint 服務站監視器，都可以分割成兩部工作站畫面。 如需最佳的分割畫面使用者體驗，建議使用最小1600x900 解析度的寬螢幕。 也建議您不要使用數位板的迷你鍵盤，讓這兩個鍵盤放在監視器前方。  
  
若要建立分割畫面電臺，您可以設定一個直接連接視頻或 USB-零用戶端連線的工作站。 然後藉由將鍵盤和滑鼠插入連接到伺服器的 USB 集線器，來新增額外的工作站中樞。 接著，您可以使用 MultiPoint 管理員分割畫面，並將新的中樞對應至一半的監視器，將站轉換成兩個工作站。 畫面的左半部會變成一個工作站，右半部則變成第二個工作站。  
  
在工作站被分割之後，一位使用者可以在另一位使用者登入正確的工作站時，登入左側的工作站。  
  
![分割畫面工作站](./media/WMS_diagram3.gif)  
  
**圖 4**含有分割螢幕電臺的 MultiPoint 服務系統  
  
## <a name="BKMK_StationTypeComparison"></a>工作站類型比較  
  
||直接連接影片|USB 零用戶端連線|已連線的 RDP over LAN|  
|-|--------------------------|-----------------------------|----------------------------|  
|影片效能|建議最佳的影片效能||使用支援 RemoteFX 的瘦用戶端，以較低的網路頻寬改善視頻品質|  
|實體限制|受限於視頻纜線長度和 USB 集線器和纜線長度（建議使用15計量長度上限）|受限於 USB 集線器和纜線長度（建議使用15計量長度上限）|受 LAN 散發限制|  
|允許的工作站數目 |受限於主機板上的可用 PCIe 插槽數乘以每張視頻的影片埠|總數目可能受到 USB 零用戶端製造商的限制（如需詳細資訊，請參閱此表格後面的注意事項）。|受限於網路交換器上的可用埠|  
|分割畫面|是|是|否|  
|MultiPoint 管理站的周邊狀態、自動登入設定、工作站重新命名|是|是|否|  
|存取伺服器啟動功能表|是|否|否|  
  
> [!NOTE]  
> 連線到伺服器的 USB 零用戶端總數可能受限於執行 MultiPoint 服務之電腦的製造商或硬體功能。
