---
title: MultiPoint 服務網站規劃
description: Windows Server 2016 中 MultiPoint 服務部署的規劃資訊
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 063783cd-d748-489e-b175-46eadc993f7a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 744e49f47f7144dac82dbe68c885060b0c08490d
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371868"
---
# <a name="multipoint-services-site-planning"></a>MultiPoint 服務網站規劃
您應該考慮將會部署一或多部執行 MultiPoint 服務之電腦及其相關聯的工作站的位置。  
  
執行 MultiPoint 服務角色的電腦應該可以方便存取電源供應器，以及直接連線到它的週邊裝置，例如印表機。 此外，執行 MultiPoint 服務的電腦必須具有網路連線的便利存取權。 必須要有網路連線，才能存取網際網路及可用的 LAN。  
  
需要考慮的其他因素包括下列各項：  
  
-   MultiPoint 服務系統會在特定的房間內設定，還是會在滾動購物車或資料表上設定，以便從位置移動？  
  
    > [!NOTE]  
    > 如果您打算使用行動裝置設定，您可以在每次重新連接時*將*工作站與 MultiPoint 服務建立關聯，以確定每個鍵盤和滑鼠都與適當的監視器相關聯。  
  
-   主要工作站會位於其他工作站旁，還是會分開？ 例如，如果在教室中設定了 MultiPoint 服務系統，主要工作站會位於老師的桌面上，而標準電臺會放在室內的其他地方嗎？ 當執行 MultiPoint 服務的電腦重新開機時，主要工作站會有啟動畫面的存取權。 如果您在意教室設定的此存取層級，您可能會想要將主要工作站放在老師的桌面上。  
  
-   房間內可以容納多少電臺？  
  
-   您需要網路嗎？ 使用直接連接視頻或 USB 零用戶端連線的單一伺服器解決方案，不需要網路。  
  
-   房間內是否有足夠的網路連線可支援執行 MultiPoint 服務所需的電腦數目  
  
-   電源輸出位於何處？  
  
-   您是否需要額外的顯示裝置，例如投影機？ 如果您打算使用投影機，它會從上限停止，還是放在資料表上？  
  
-   需要何種纜線，以及需要多少纜線？  
  
-   請考慮您未來可能會想要擴充的方式。 您會新增更多工作站嗎？  
  
## <a name="station-layout-and-configuration"></a>工作站版面配置和設定  
網站的實體版面配置可能會影響您選擇的工作站類型。 如需不同工作站類型的詳細資訊，請參閱本指南中的[MultiPoint 電臺](MultiPoint-services-Stations.md)。 單一 MultiPoint 服務上允許多部工作站類型。 這可為您提供額外的彈性，以符合您的安裝需求。  
  
### <a name="layout-for-direct-video-connected-stations"></a>直接連接視頻的工作站版面配置  
  
-   若為直接連接視頻的站，監視器與電腦之間的距離會受到視頻纜線長度的限制。  
  
-   支援使用中繼集線器或菊輪鍊站中樞進行輕鬆部署，但最大建議的連續中樞數目為三個。 這表示從電腦到工作站中樞的最大距離為15計量，因為每個 USB 2.0 纜線的最大長度為五個計量。  
  
> [!IMPORTANT]  
> 每部電腦應一律至少有一個直接視頻連線站，以作為主要工作站。  
  
### <a name="layout-for-usb-zero-client-connected-stations"></a>USB 零用戶端連線工作站的版面配置  
  
-   支援使用中繼集線器或菊輪鍊站中樞進行輕鬆部署，但最大建議的連續中樞數目為三個。 這表示從電腦到工作站中樞的最大距離為15計量，因為每個 USB 2.0 纜線的最大長度為五個計量。  
  
-   連接到單一中繼中樞的最大建議 USB 零用戶端數目為三個。  
  
    > [!NOTE]  
    > 有些電腦會隨附于主機板上的一般集線器，其作用是在電腦的*根中樞*與站中樞之間新增額外的中樞。  
  
-   如果將大量使用影片，建議您將兩個以上的 USB 零用戶端連接到伺服器上的 USB 埠。 例如，如果使用中繼中樞，則只有兩個 USB 零用戶端連線。 或者，如果您是透過菊輪鍊 USB 零用戶端，則只有兩個 USB 零用戶端應該連結在一起。 將每個 USB 零用戶端新增至伺服器上的 USB 埠，可減少可用的影片頻寬。  
  
-   如果您打算將三個以上的 USB 零用戶端連接到伺服器上的單一 USB 埠，建議使用伺服器和中繼中樞之間的 USB 3.0。  
  
> [!NOTE]  
> 建議您使用您的應用程式和硬體來決定您可以連接到伺服器上的 USB 埠多少 USB 零用戶端，以驗證效能。  
  
![下游集線器](./media/WMS_diagram6.gif)  
  
**圖 5**MultiPoint 服務系統，其中包含三個連接至單一中繼中樞的 USB 零用戶端  
  
### <a name="layout-for-rdp-over-lan-connected-stations"></a>RDP over LAN 連線工作站的版面配置  
LAN 用戶端沒有實體距離限制。 只要它們在 LAN 上，就可以連接到 MultiPoint 服務系統。  
  
## <a name="using-additional-hubs"></a>使用其他中樞  
您可以使用其他中樞來簡化安裝作業。 MultiPoint 服務系統上使用三種類型的中樞：  
  
-   [站中樞](#station-hubs)  
  
-   [中繼中樞](#intermediate-hubs)  
  
-   [下游中樞](#downstream-hubs)  
  
### <a name="station-hubs"></a>站中樞  
「工作站中樞」是一種與 MultiPoint 服務站相關聯的外部集線器。 最少的是，工作站中樞會將鍵盤插入其中。 它也可能附加額外的週邊設備。 工作站中樞可以是符合 USB 2.0 或更新版本規格的一般 USB 集線器。 如果具有高電源的裝置將會對其進行外掛程式，則工作站中樞應為外部電源。  
  
**根中樞**電腦主機板上的主機控制器內建的 USB 集線器稱為「*根集線器*」。 在執行 MultiPoint 服務的電腦上，通常會將工作站集線器插入根中樞。  
  
> [!NOTE]  
> 根中樞不應做為工作站中樞使用。 當 USB 埠內建至電腦時，通常無法判斷內部連線到的 USB 根集線器。 因此，如果您將工作站鍵盤與滑鼠直接插入電腦的 USB 埠，實際上可能是將鍵盤和滑鼠插到不同的 USB 根集線器。 若要確保鍵盤和滑鼠位於相同的中樞，請在電腦的 USB 埠上插入站中樞，然後將鍵盤和滑鼠插到該工作站中樞。  
  
**菊輪鍊工作站**將工作站集線器連接到另一個工作站中樞，而不是直接連線到電腦，可能會比較容易。 這可讓您將 USB 集線器連接到已插入電腦的工作站中樞，以便將工作站集線器連接到另一個站中樞。  
  
不能連續出現三個以上的 USB 零用戶端或工作站中樞菊輪鍊。 請注意，當菊輪鍊式站中樞時，不會超過 USB 頻寬。  
  
![菊輪鍊工作站](./media/WMS_diagram5.gif)  
  
**圖 6**具有菊輪鍊式站的 MultiPoint 服務系統  
  
### <a name="intermediate-hubs"></a>中繼中樞  
中繼中樞是伺服器與工作站中樞之間的中樞。 它通常用來增加可用於工作站中樞的埠數目，或延長工作站與電腦之間的距離。 建議在站中樞和伺服器之間使用兩個以上的中繼中樞。  
  
中繼集線器必須是 USB 2.0 或更新版本，而且必須在外部供電。 如果您要將三個以上的 USB 零用戶端連接到中繼中樞，建議您在伺服器和中繼中樞之間使用 USB 3.0。  
  
### <a name="downstream-hubs"></a>下游集線器  
下游中樞會連線到站中樞，為工作站裝置新增更多可用的埠。 下游中樞可以是外部電源或匯流排驅動，視插入集線器的裝置而定。  
  
![多個 USB 零用戶端連接](./media/WMS_diagram4.gif "WMS_diagram4")  
  
**圖 7**具有中繼中樞、站中樞和下游中樞的 MultiPoint 服務系統  
  
## <a name="users-stations-and-computers"></a>使用者、工作站和電腦  
您需要的工作站數目取決於需要同時存取執行 MultiPoint 服務之電腦的人數。 同樣地，執行 MultiPoint 服務的電腦數目也必須視所需的總數量而定。 直接連接視頻的工作站、USB-零用戶端連線的工作站，以及透過 LAN 連線的工作站，全都被視為站。 此外，如果使用分割畫面功能，則每一半都會被視為一站。  
  
## <a name="power-considerations"></a>電源考慮  
下列元件需要存取 power 帶狀或插座：  
  
-   Server  
-   監視器
-   中繼中樞 \(（如果使用）\) 
-   某些 USB 零用戶端  
-   電源上的 USB 裝置，例如一些外部存放裝置和 DVD 光碟機  
  
## <a name="sample-multipoint-services-system-layouts"></a>MultiPoint 服務系統版面配置範例  
根據可用的傢俱、房間的大小、執行 MultiPoint 服務的電腦數目，以及房間中的工作站，有各種方式可以安排實體電臺的順序。 下圖說明五個可能的替代方案。  
  
> [!NOTE]  
> 其中一些圖表會顯示連接到 MultiPoint 服務系統的投影機。 這只是範例;在 MultiPoint 服務系統中包含投影機是選擇性的。  
  
**電腦實驗室**在此設定中，工作站會沿著房間的牆數排列，而學生朝向牆。  
  
![電腦實驗教室設定](./media/WMS_ComputerLabLayout.gif)  
  
**群組**在此設定中，有三部執行 MultiPoint 服務的電腦，其中的工作站會在每部電腦上叢集化。  
  
![使用伺服器 Pod 設定的教室](./media/WMS_ClassroomPods.gif)  
  
**演講室**在此設定中，會在資料列中設定工作站。 這項設定的優點在於，所有學生都面臨講師。  
  
![設定為演講廳的教室](./media/WMS_LectureRoom.gif)  
  
**活動中心**這項設定是由桌上型電腦的傳統教室配置所組成，而且有一個獨立的區域，其中包含一部執行 MultiPoint 服務及其相關聯站的電腦。  
  
![MultiPoint 服務活動中心](./media/WMSActivityCenter.gif)  
  
**小型企業辦公室**在此設定中，執行 MultiPoint 服務的電腦會放在中央位置，而整個辦公室的使用者會使用區域網路 \(LAN\)來連接到該區域。  
  
![USB 極簡型用戶端連線的工作站](./media/Diagram1.gif)